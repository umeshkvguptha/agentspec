# AgentSpec

> **The spec format AI coding agents can actually consume — and the linter that enforces it.**

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](./LICENSE)
[![Status: v0.1](https://img.shields.io/badge/status-v0.1-green.svg)]()
[![Runs in browser](https://img.shields.io/badge/runs-in%20browser-orange.svg)]()
[![Zero dependencies](https://img.shields.io/badge/dependencies-zero-brightgreen.svg)]()

**Live editor → https://umeshkvguptha.github.io/agentspec**
**Source → https://github.com/umeshkvguptha/agentspec**

---

## The problem with English specs

Feed the same English ticket to the same model. Do it with two different teams. Get two completely different codebases.

The variance isn't the model. **It's the spec.**

English tickets skip the boring parts. Acceptance criteria are vague. The single most important field — *what NOT to do* — is almost never written down. So agents wander into adjacent features, hallucinate APIs, skip error handling, and you spend your code-review budget pruning instead of approving.

OpenAPI fixed this for HTTP. **There is no equivalent for AI-driven feature work.**

Until now.

---

## What AgentSpec is

AgentSpec is the smallest opinionated structure that closes the gap between "a human described something" and "an agent can act on it reliably."

Three things set it apart:

**1. A schema with required fields the English version always skips**

Not just title and description. Enumerated error modes. Measurable acceptance criteria. Explicit `out_of_scope`. Invariants that hold across all paths. Context pointers to existing symbols so the agent doesn't reinvent them.

**2. A linter that catches what wrecks agent output**

Every spec gets a **0–100 quality score**. The linter fires on:
- Vague language — `"fast"`, `"secure"`, `"improve"` without numbers
- Scope leakage — `"etc."`, `"and so on"`, `"..."` (agents hallucinate the rest)
- Missing error modes — contracts that only define the happy path
- Untestable acceptance — `"then: works correctly"` is not measurable
- Empty `out_of_scope` — without an explicit fence, agents wander
- Missing required fields — caught before the agent ever sees it

**3. One spec → multiple agent targets**

Write once. Compile to a Claude Code prompt brief, a Markdown spec for your reviewer, canonical YAML, or JSON. Same input, whichever agent your team uses this quarter.

---

## Schema at a glance

| Field | Required | Why it exists |
|---|---|---|
| `id`, `title`, `intent` | **yes** | Anchors the agent's work. Vague title = vague code. |
| `contracts` | **yes** | The interfaces the agent must implement. Errors enumerated by name, not implied. |
| `acceptance` (given/when/then) | **yes** | Doubles as a test plan. Every `then` clause must be measurable. |
| `invariants` | recommended | Properties that hold across all paths — security, audit, data integrity. |
| `out_of_scope` | recommended | The most underused field in human specs. Prevents adjacent-feature drift. |
| `context` (files, symbols) | recommended | Existing code the agent should know about — so it doesn't reinvent it. |
| `constraints` | recommended | Performance, security, dependency limits — **with numbers**. |

---

## Before AgentSpec vs. After

**Before — a typical English ticket:**
```
Add a user login flow. It should be fast and secure.
Make sure errors are handled properly.
```
Result: the agent builds its own auth library, adds password reset (not asked for), skips rate limiting, and "handles errors" with a generic 500.

**After — an AgentSpec:**
```yaml
id: auth-login-v1
title: "User login: credential validation and session creation"
intent: "Authenticate an existing user by email + password, issue a JWT, reject on failure."
contracts:
  - name: POST /auth/login
    inputs: [email, password]
    outputs: [jwt_token, expires_at]
    errors: [INVALID_CREDENTIALS, ACCOUNT_LOCKED, RATE_LIMITED]
acceptance:
  - given: "valid credentials"
    when: "POST /auth/login"
    then: "returns 200 with jwt_token, expires_at in 3600s"
  - given: "wrong password 5× in 60s"
    when: "POST /auth/login"
    then: "returns 429, account locked for 15 minutes"
out_of_scope:
  - password reset
  - OAuth / social login
  - email verification
constraints:
  - "p95 response time < 200ms under 500 rps"
  - "no new auth libraries — use existing bcrypt + jose"
```
Result: the agent implements exactly that, nothing more.

---

## Run locally

```bash
git clone https://github.com/umeshkvguptha/agentspec.git
cd agentspec
python3 -m http.server 8000
# open http://localhost:8000
```

No build step. No dependencies. The entire app is a single self-contained `index.html`.

---

## Privacy guarantee

Zero network requests at runtime. Open DevTools → Network. Edit a spec, lint it, compile it. Watch the tab stay empty. The whole app runs in your browser. Theme preference is stored in `localStorage` — local only, never sent anywhere.

---

## Deploy your own (free, ~60 seconds)

This repo ships with a GitHub Actions workflow that publishes to GitHub Pages on every push to `main`.

1. Push the repo to your GitHub account.
2. **Settings → Pages → Build and deployment → Source: GitHub Actions**
3. Push to `main`. Live in ~60 seconds.

---

## Roadmap

| Item | Stage |
|---|---|
| Cursor `.cursorrules` + Aider system-prompt compilers | v0.2 |
| VS Code extension — inline linting on `.agentspec.yaml` files | v0.2 |
| GitHub Action — lint AgentSpec files in PRs, block on score drop, comment diff | v0.3 |
| Spec-to-PR provenance — every commit links back to the spec hash that produced it | v0.3 |
| Test scaffolding — generate Jest / pytest skeletons from acceptance scenarios | v0.4 |
| **Enterprise governance tier** — shared spec libraries, custom linter rules, audit trails for regulated AI-SDLC (banking, brokerage, healthcare) | planned |

The enterprise tier is the reason this exists. If you'd buy it, [open an issue](https://github.com/umeshkvguptha/agentspec/issues) or DM on LinkedIn.

---

## Who this is for

- **Engineering teams shipping features with Claude Code, Cursor, or Aider** and tired of reviewing agent output that drifted from the ticket.
- **Tech leads** who want a structured spec review step before handing work to an agent.
- **Regulated industries** (banking, brokerage, healthcare) where auditability of AI-generated code is non-negotiable.

---

## Built by

[Umesh Kumar K V](https://www.linkedin.com/in/umeshkvguptha/) — built while delivering a broker-dealer trading platform via agentic SDLC. 20+ years across FIS, Caesars Sportsbook, and Wissen Technology. Anthropic-certified in MCP and Claude Code.

---

## License

MIT — see [LICENSE](./LICENSE).
