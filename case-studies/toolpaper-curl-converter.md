# Engineering Case Study: Toolpaper (`curl-converter`)

**Live Product:** [toolpaper.vercel.app](https://toolpaper.vercel.app/)  
**Category:** Browser-First Developer Utility · Compiler/Transpiler Pipeline · Request Sanitization  
**Source Status:** Private application repository (this document details the parser architecture, intermediate representation, code generators, and test suite).

---

## 1. Problem & Product Goal

When developers copy network requests from browser DevTools (*"Copy as cURL"*), the resulting command is rarely ready for production code:
1. **Shell dialect differences:** Chrome/Edge on Windows emit `cmd.exe` (`^` continuations) or PowerShell (backtick continuations), while macOS/Linux emit Bash (`\` continuations and `$'...'` ANSI-C quoting).
2. **DevTools header bloat:** Browsers inject 20+ automatic headers (`sec-ch-ua*`, `sec-fetch-*`, `upgrade-insecure-requests`, session-conditional `if-none-match`) that clutter generated code and can break server-side replay.
3. **Silent flag loss in naive converters:** Most online cURL converters silently drop unsupported flags or shell interpolations (`$(...)`, `$TOKEN`), producing code that fails subtly at runtime.
4. **Credential leakage when sharing:** Developers paste raw cURL commands containing Bearer tokens, API keys, or session cookies into tickets or share links.

**Toolpaper** is a client-side cURL workbench that parses, cleans, explains, and transpiles cURL commands across **18 language and library targets**, while surfacing explicit parser warnings and redacting secrets before sharing.

---

## 2. Pipeline Architecture

All parsing, normalization, static analysis, and code generation execute 100% locally in the browser using a pure TypeScript pipeline (`src/lib/curl-parser.ts` and `src/lib/curl-cleanup.ts`).

```mermaid
flowchart LR
    Input["Raw cURL Input\n(Bash · PowerShell · cmd)"] --> Dialect["Dialect Detector\n& Tokenizer"]
    Dialect --> IR["Normalized ParsedCurl IR\n+ Structured Warnings"]
    IR --> Cleanup["Static Analyzer\n(Noise Headers & Secret Scanner)"]
    IR --> Matrix["Capability Verifier\n(Target Support Matrix)"]
    Cleanup --> Gen["18 Code Generators\n(JS, Python, Go, Rust, etc.)"]
    Matrix --> Gen
    IR --> Explain["Request Explainer\n& Optional Browser Runner"]
```

### 1. Dialect Detection & Shell-Aware Tokenizer (`src/lib/curl-parser.ts`)
Instead of splitting on whitespace or regex-matching flags, the parser runs a two-stage front end:
- **Dialect detection (`detectDialect`):** Identifies PowerShell (backtick line continuations), Windows `cmd.exe` (`^` continuations), or POSIX Bash (`\` continuations).
- **Lexer (`tokenize`):** Folds dialect-specific line continuations, strips leading `$` on ANSI-C `$'...'` strings while preserving escape sequences (`\n`, `\t`, `\"`, `\\`), and handles single, double, and unquoted glued flags (such as `-XPOST` or `-HAuthorization:Bearer...`).
- **Interpolation detection:** Scans tokens for unexpanded shell variables (`$VAR`, `${VAR}`, `$(cmd)`) and emits structured `Warning` diagnostics rather than pretending to evaluate shell state.

### 2. Normalized Intermediate Representation (`ParsedCurl`)
Tokens are lowered into a strongly typed `ParsedCurl` IR:

```ts
export type ParsedCurl = {
  method: string;
  url: string;
  headers: Record<string, string>;
  body: string | null;
  form: FormField[];
  isGet: boolean;
  auth: string | null;
  authScheme: "basic" | "digest" | "ntlm" | "negotiate" | "bearer" | null;
  insecure: boolean;
  cookies: string | null;
  cookieFile: string | null;
  cookieJar: string | null;
  dialect: "bash" | "powershell" | "cmd";
  warnings: Warning[];
  followRedirects: boolean;
  compressed: boolean;
  timeoutMs: number | null;
  connectTimeoutMs: number | null;
  retries: number | null;
  httpVersion: "1.0" | "1.1" | "2" | "3" | null;
  proxy: string | null;
  failOnError: boolean;
  outputFile: string | null;
  head: boolean;
  tlsMinVersion: "1.0" | "1.1" | "2" | "3" | null;
  clientCert: string | null;
  caCert: string | null;
};
```

### 3. DevTools Noise Classification & Secret Redaction (`src/lib/curl-cleanup.ts`)
Two pure static-analysis passes inspect `ParsedCurl`:
- **`analyzeNoise(parsed)`:** Classifies browser-injected headers into six categories (`client-hint`, `fetch-metadata`, `navigation`, `default`, `cache`, and `review`). Headers in `client-hint` (`sec-ch-ua*`) or `fetch-metadata` (`sec-fetch-*`) are recommended for automatic stripping, whereas `origin` and `referer` are marked `review` because server CSRF/CORS policies sometimes require them.
- **`analyzeSecrets(parsed)`:** Scans headers, cookies, URL query parameters, JSON body fields, and basic-auth credentials against anchored credential signatures (Bearer JWTs, Stripe keys, GitHub PATs, AWS keys, Slack tokens, high-entropy session cookies). Returns stable `SecretFinding` records with redacted previews so users can one-click redact secrets before copying code or creating a `#` share link.

### 4. Target Code Generation & Capability Matrix (`src/lib/capability.ts`)
From `ParsedCurl`, 18 dedicated target generators emit idiomatic request code:
- **JavaScript / Node.js:** `fetch`, `axios`, `ky`, Node native `fetch`
- **Python:** `requests`, `httpx`, `aiohttp`
- **Go:** `net/http`
- **Rust:** `reqwest`
- **PHP:** `ext-curl`, `Guzzle`
- **JVM / .NET / Apple:** Java `OkHttp`, Kotlin `OkHttp`, C# `HttpClient`, Swift `URLSession`
- **Shell / CLI:** PowerShell `Invoke-RestMethod`, `HTTPie`, normalized `cURL`

When a parsed option (e.g., `--tlsv1.3`, `--cert`, or `--retry`) cannot be expressed natively in a given target library without custom transport boilerplate, the capability verifier emits an explicit warning or inline comment rather than silently omitting the constraint.

---

## 3. Quality Assurance & Test Coverage

The core engine is verified with **Vitest** unit and property-style test suites (`src/lib/*.test.ts`):
- **`curl-parser.test.ts`:** Tests dialect detection, ANSI-C quoting, glued short flags, `-G` query-string migration, `--data-urlencode`, multipart `-F` file/field distinction, auth schemes, and warning emission across all 18 output generators.
- **`curl-cleanup.test.ts`:** Verifies DevTools noise categorization, non-destructive `review` header preservation, secret detection across headers/cookies/JSON bodies, and idempotent redaction.
- **`capability.test.ts` & `capability-verification.test.ts`:** Cross-checks target capability declarations against actual generator outputs so unsupported flags are always surfaced to the user.
- **`line-diff.test.ts` & `highlight.test.ts`:** Verifies the cleanup diff viewer and Prism syntax highlighting tokenization.

---

## 4. Honest Limitations

- **Not a POSIX shell emulator:** Subshells (`$(...)`), environment variable expansions, and local filesystem reads (`-d @file.json`) are flagged with warnings rather than executed.
- **Browser `Run` panel constraints:** The optional interactive request runner uses browser `fetch()`, which is subject to CORS, browser-forbidden headers, and mixed-content restrictions.
- **Reversible share links:** Share links store the command in the URL fragment (`#`) so data never hits a server, but anyone given the link can decode the fragment—hence the built-in secret scanner warning before link creation.
