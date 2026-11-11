<picture><source media="(prefers-color-scheme: dark)" srcset="assets/2-paths-dark.png"><source media="(prefers-color-scheme: light)" srcset="assets/1-paths-light.png"><img src="assets/1-paths-light.png" alt="An illustrated landscape of glass, ceramic and amber paths." width="100%"></picture>

# Hi, I'm Manav.

I'm a Mechanical Engineering student and **product-minded software developer** building software that makes complex workflows simpler and more reliable.

I primarily build **interactive learning products** and **browser-first developer utilities**, with a focus on client-side state architecture, input parsing, and automated verification. Alongside web engineering, I run empirical **reinforcement learning** experiments.

**[Portfolio](https://manavdev.site/)** · **[LinkedIn](https://www.linkedin.com/in/manav-karhale)** · **[Email (Hi@manav4u.tech)](mailto:Hi@manav4u.tech)** · **[X (@ManavShips)](https://x.com/ManavShips)**

---

## Featured Engineering Work

### 1. [PrepTracker](https://github.com/manav4u/PrepTracker) · Academic Planning & Spaced-Repetition Web App
A zero-account study tracker and spaced-review workspace for SPPU engineering students.
- **Architecture & state:** Browser-local persistence (`localStorage`) backed by schema-validated JSON backup/restore (`v3.1.0` → `v4.2.0` migration and one-click rollback) plus a pure spaced-repetition queue derived directly from syllabus progress and activity logs.
- **Verification:** 114 unit assertions across 14 Node native TypeScript test suites and 11 Playwright E2E / `axe-core` accessibility specs running in GitHub Actions.
- **Links:** [Live App](https://preptracker.manavdev.site/) · [Source Code](https://github.com/manav4u/PrepTracker) · [Architecture & ADRs](https://github.com/manav4u/PrepTracker/blob/main/docs/architecture/README.md) · [Latest Release](https://github.com/manav4u/PrepTracker/releases/tag/v0.26.10.20)

### 2. [Git, Visually (`gg-buddy`)](case-studies/git-visually.md) · Interactive Git Course & Browser Terminal Sandbox
A 15-chapter visual Git and GitHub course with an in-browser practice terminal, interactive concept replicas, and a 243-command searchable cheat sheet.
- **Architecture & state:** Custom 22-command in-browser Git state machine (virtual working tree, staging index, branch pointers, and live commit DAG) verified against 61 real `git` CLI outputs; zero-backend cross-device progress portability using shareable `#gv=` snapshots and monotonic state merging; network-first offline PWA service worker.
- **Links:** [Live Course](https://git.manavdev.site/) · [Engineering Case Study & Test Fixtures](case-studies/git-visually.md) *(application source is private)*

### 3. [Toolpaper (`curl-converter`)](case-studies/toolpaper-curl-converter.md) · Browser-Native cURL Transpiler & Sanitizer
A client-side developer workbench that parses, cleans, explains, and converts cURL commands into request code across **18 language and library targets**.
- **Architecture & analysis:** Shell-aware lexer (Bash, PowerShell, and `cmd.exe` continuations, ANSI-C `$'...'` strings, glued short flags) lowering to a typed `ParsedCurl` intermediate representation; static analyzer that classifies 25+ browser-generated headers, preserves server-sensitive `origin`/`referer` headers for review, and runs heuristic credential detection before sharing.
- **Links:** [Open Toolpaper](https://toolpaper.vercel.app/) · [Engineering Case Study & Parser Vectors](case-studies/toolpaper-curl-converter.md) *(application source is private)*

### 4. [LunarLander-Reward-Shaping](https://github.com/manav4u/LunarLander-Reward-Shaping) · Empirical PPO Reinforcement Learning Study
An empirical study of **reward hacking (specification gaming)** vs. **potential-difference reward shaping** on Gymnasium's `LunarLander-v3` using Stable-Baselines3 PPO.
- **Experiment & evaluation:** Compares an untrained baseline, standard PPO (`269.41 ± 14.90`), a flawed airborne-hover bonus (`-56.07 ± 20.20` true task score), and a frame-to-frame potential shaping wrapper (`ΔΦ = Φ(sₜ) − Φ(sₜ₋₁) − 0.05`). All checkpoints are evaluated with training wrappers disabled, including a 500-episode deterministic benchmark (`274.98` mean reward, `495/500` episodes `≥ 200`).
- **Links:** [Source, Checkpoints & Evaluation CLI](https://github.com/manav4u/LunarLander-Reward-Shaping)

---

## Open-Source Contributions

**Merged**
- **[`rapidfuzz/RapidFuzz#505`](https://github.com/rapidfuzz/RapidFuzz/pull/505)** — Honor falsey callable `processor` arguments in `cdist` and `cpdist` across Cython and Python fallback paths, with 30 regression test cases.
- **[`tree-sitter-python-orchard#15`](https://codeberg.org/grammar-orchard/tree-sitter-python-orchard/pulls/15)** — Give bitwise `AND` higher operator precedence than `XOR` in the Python Tree-sitter grammar *(merged on Codeberg)*.
- **[`jquast/wcwidth#316`](https://github.com/jquast/wcwidth/pull/316)** *(via [`#310`](https://github.com/jquast/wcwidth/pull/310))* — Preserve terminal SGR styles when dropping whitespace-only wrapped lines *(maintainer-merged PR incorporating five commits from `#310` with co-author credit)*.

**Currently Under Review**
- **[`Quantinuum/guppylang#2517`](https://github.com/Quantinuum/guppylang/pull/2517)** — Preserve qualified names for `comptime` functions in diagnostics and type representations.
- **[`lepture/mistune#552`](https://github.com/lepture/mistune/pull/552)** — Python 3.15 regex scanner compatibility fix.

**Issue Investigation**
- **[`colinhacks/zod#6743`](https://github.com/colinhacks/zod/issues/6743)** — Reproduced and traced a false boolean-schema `$ref` resolution bug in JSON Schema conversion, providing a minimal reproduction and proposed regression test.

---

## Technical Capabilities

| Area | Technologies & Practices | Demonstrated In |
| :--- | :--- | :--- |
| **Frontend & UI Engineering** | React 19, TypeScript, Vite, Tailwind CSS, Astro, PWA Service Workers | [PrepTracker](https://github.com/manav4u/PrepTracker), [Git, Visually](case-studies/git-visually.md), [Toolpaper](case-studies/toolpaper-curl-converter.md) |
| **Parsing & Client Architecture** | Shell-aware tokenization, typed intermediate representations, static analysis, `localStorage` schema migration, URL-fragment state portability | [Toolpaper](case-studies/toolpaper-curl-converter.md), [PrepTracker](https://github.com/manav4u/PrepTracker), [Git, Visually](case-studies/git-visually.md) |
| **Testing & Quality Assurance** | Node.js native test runner, Vitest, Playwright E2E, `axe-core` accessibility audits | [PrepTracker](https://github.com/manav4u/PrepTracker), [Git, Visually](case-studies/git-visually.md), [Toolpaper](case-studies/toolpaper-curl-converter.md) |
| **Machine Learning / RL** | Python, Gymnasium (`Box2D`), Stable-Baselines3 (PPO), reward wrapper design & unwrapped evaluation | [LunarLander-Reward-Shaping](https://github.com/manav4u/LunarLander-Reward-Shaping) |
| **Workflow & Delivery** | Git, GitHub Actions CI/CD, static pre-rendering, release versioning | [PrepTracker](https://github.com/manav4u/PrepTracker), [LunarLander-Reward-Shaping](https://github.com/manav4u/LunarLander-Reward-Shaping), [Git, Visually](case-studies/git-visually.md), [Toolpaper](case-studies/toolpaper-curl-converter.md) |

---

## Currently

Building browser-first developer tools, deepening software engineering fundamentals, and contributing bug fixes to established open-source projects.

## How I Work

I optimize for understandable architecture, predictable state management, and reliable user-data handling. I use AI-assisted development selectively, validating implementation decisions through deterministic tests and edge-case analysis.
