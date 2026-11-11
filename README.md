<picture><source media="(prefers-color-scheme: dark)" srcset="assets/2-paths-dark.png"><source media="(prefers-color-scheme: light)" srcset="assets/1-paths-light.png"><img src="assets/1-paths-light.png" alt="An illustrated landscape of glass, ceramic and amber paths." width="100%"></picture>

# Hi, I'm Manav.

I'm a Mechanical Engineering student and **product-minded software developer** who turns complex workflows into clear, reliable software.

I primarily build **interactive learning products** and **browser-first developer utilities**, with a focus on client-side state design, static analysis/parsing, and automated verification. Alongside web engineering, I run empirical **reinforcement learning** experiments.

**[Portfolio](https://manavdev.site/)** · **[LinkedIn](https://www.linkedin.com/in/manav-karhale)** · **[Email (Hi@manav4u.tech)](mailto:Hi@manav4u.tech)** · **[X (@ManavShips)](https://x.com/ManavShips)**

---

## Featured Engineering Work

### 1. [PrepTracker](https://github.com/manav4u/PrepTracker) · Academic Planning & Spaced-Repetition Web App
A zero-account study tracker and spaced-review workspace built for SPPU engineering students.
- **Engineering highlights:** React 19 + TypeScript + Vite architecture with static pre-rendering, deterministic spaced-repetition scheduling (`[3, 7, 21]` day intervals with daily queue caps and shaky-recall handling), and schema-versioned (`3.1.0` → `4.2.0`) JSON backup/restore with preview and one-click rollback.
- **Verification & CI:** 14 Node native TypeScript unit test suites + 11 Playwright E2E and `axe-core` accessibility specs running in GitHub Actions prior to GitHub Pages deployment.
- **Links:** [Live App](https://preptracker.manavdev.site/) · [Source & Architecture](https://github.com/manav4u/PrepTracker) · [Latest Release](https://github.com/manav4u/PrepTracker/releases/tag/v0.26.10.20)

### 2. [Git, Visually (`gg-buddy`)](case-studies/git-visually.md) · Interactive Git Course & Browser Terminal Sandbox
A 15-chapter visual Git and GitHub course with an in-browser practice terminal, interactive concept replicas, and a 243-command searchable cheat sheet.
- **Engineering highlights:** Custom 22-command in-browser Git state machine (virtual working tree, staging index, and live commit DAG) verified against 61 real `git` CLI outputs; zero-backend cross-device progress sync using URL-safe Base64 hash fragments (`#gv=`) with monotonic high-water-mark merging; network-first offline PWA service worker.
- **Verification:** Driven across desktop (`1366×768`) and mobile touch (`390px`) viewports via Playwright E2E and accessibility tests.
- **Links:** [Live Course](https://git.manavdev.site/) · [Read the Engineering Case Study](case-studies/git-visually.md) *(application source is private)*

### 3. [Toolpaper (`curl-converter`)](case-studies/toolpaper-curl-converter.md) · Browser-Native cURL Transpiler & Sanitizer
A client-side developer workbench that parses, cleans, explains, and converts cURL commands into idiomatic request code across **18 language and library targets**.
- **Engineering highlights:** Dialect-aware lexer (Bash, PowerShell backticks, and `cmd.exe` `^` continuations, ANSI-C `$'...'` strings, glued short flags) lowering to a typed `ParsedCurl` intermediate representation; static analyzer that strips 25+ DevTools noise headers (`sec-ch-ua*`, `sec-fetch-*`) and detects leaked credentials (Bearer tokens, API keys, session cookies) before sharing; explicit capability matrix that warns when flags cannot be represented in a target library.
- **Links:** [Open Toolpaper](https://toolpaper.vercel.app/) · [Read the Engineering Case Study](case-studies/toolpaper-curl-converter.md) *(application source is private)*

### 4. [LunarLander-Reward-Shaping](https://github.com/manav4u/LunarLander-Reward-Shaping) · Empirical PPO Reinforcement Learning Study
An empirical study of **reward hacking (specification gaming)** vs. **potential-difference reward shaping** on Gymnasium's `LunarLander-v3` using Stable-Baselines3 PPO.
- **Engineering highlights:** Compares an untrained baseline, standard PPO (`269.41 ± 14.90`), a flawed airborne-hover bonus that induces reward hacking (`-56.07 ± 20.20` true task score), and a frame-to-frame potential shaping wrapper (`ΔΦ = Φ(sₜ) − Φ(sₜ₋₁) − 0.05`). All checkpoints are evaluated strictly with training wrappers disabled, including a 500-episode deterministic benchmark (`274.98` mean reward, `495/500` episodes `≥ 200`) with paired bootstrap confidence intervals.
- **Links:** [Source, Checkpoints & Rollout Videos](https://github.com/manav4u/LunarLander-Reward-Shaping)

---

## Open Source & Upstream Investigation

- **[`colinhacks/zod#6743`](https://github.com/colinhacks/zod/issues/6743) — JSON Schema `$ref` resolution bug report:** Investigated a specific reference-resolution defect in Zod's JSON Schema conversion, providing a minimal deterministic reproduction, expected vs. actual output analysis, root-cause trace, and proposed regression test coverage.

---

## Technical Capabilities

Technologies listed below are backed directly by the repositories and case studies above:

| Area | Technologies & Practices | Demonstrated In |
| :--- | :--- | :--- |
| **Frontend & UI Engineering** | React 19, TypeScript, Vite, Tailwind CSS, Astro, PWA Service Workers | [PrepTracker](https://github.com/manav4u/PrepTracker), [Git, Visually](case-studies/git-visually.md), [Toolpaper](case-studies/toolpaper-curl-converter.md) |
| **Parsing & Client Architecture** | Custom tokenizers/ASTs, `localStorage` schema migration, URL-fragment state sync | [Toolpaper](case-studies/toolpaper-curl-converter.md), [PrepTracker](https://github.com/manav4u/PrepTracker), [Git, Visually](case-studies/git-visually.md) |
| **Testing & Quality Assurance** | Node.js native test runner, Vitest, Playwright E2E, `axe-core` accessibility audits | [PrepTracker](https://github.com/manav4u/PrepTracker), [Git, Visually](case-studies/git-visually.md), [Toolpaper](case-studies/toolpaper-curl-converter.md) |
| **Machine Learning / RL** | Python, Gymnasium (`Box2D`), Stable-Baselines3 (PPO), reward wrapper design & evaluation | [LunarLander-Reward-Shaping](https://github.com/manav4u/LunarLander-Reward-Shaping) |
| **Workflow & Delivery** | Git, GitHub Actions CI/CD, static pre-rendering, release versioning | All repositories |

---

## How I Work

I design software around concrete user friction—preferring zero-account local persistence where possible, explicit migration and backup paths so user data isn't lost, and automated tests to verify edge cases. I use AI-assisted workflows as force multipliers during implementation and exploration, while keeping architecture, edge-case validation, and testing grounded in deterministic code.

---

## Contact

**[Portfolio](https://manavdev.site/)** · **[LinkedIn](https://www.linkedin.com/in/manav-karhale)** · **[Hi@manav4u.tech](mailto:Hi@manav4u.tech)** · **[X (@ManavShips)](https://x.com/ManavShips)**
