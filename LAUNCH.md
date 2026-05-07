# AgentSpec — Launch Kit

## 1. Push to GitHub

Create the empty repo at **https://github.com/new**:
- Owner: `umeshkvguptha`
- Repo name: `agentspec`
- Visibility: **Public**
- Leave everything else unchecked

Then in Terminal:

```bash
cp -R "/Users/umeshkumarkotaladinnevaradaraj/Library/Application Support/Claude/local-agent-mode-sessions/23abc3d7-b5eb-4907-a306-4aef51c62553/cf84d71e-17e8-426f-a516-fd472ec08df7/local_5809de48-ea4a-4591-be33-027ff1a370e6/outputs/agentspec" ~/agentspec
cd ~/agentspec

git init -b main
git add .
git commit -m "feat: AgentSpec v0.1 — a spec format AI coding agents can consume"
git remote add origin https://github.com/umeshkvguptha/agentspec.git
git push -u origin main
```

## 2. Enable GitHub Pages

**https://github.com/umeshkvguptha/agentspec/settings/pages** → Source: **GitHub Actions**

Watch the deploy at **https://github.com/umeshkvguptha/agentspec/actions**

Live in ~60 seconds at: **https://umeshkvguptha.github.io/agentspec**

## 3. Repo metadata

Add these topics via **Settings → Topics** to maximize discoverability:

`agentic-ai` · `claude-code` · `cursor` · `aider` · `cline` · `prompt-engineering` · `mcp` · `developer-tools` · `spec-driven-development` · `linter` · `open-source`

---

## 4. LinkedIn launch post

> Post this **after** you've verified the live URL works.
> The first 3 lines are the hook — LinkedIn's "see more" cuts there on mobile. Make them land.

---

Same spec. Same model. Two teams. Completely different code.

I've watched this happen too many times to keep ignoring it.

The variance isn't the model — it's the spec. English tickets skip the boring parts. Acceptance criteria are vague. The single most important field of all — *what NOT to do* — is almost never written down.

So agents wander. They extend into adjacent features, hallucinate APIs, skip error handling. You burn your code-review budget pruning instead of approving.

OpenAPI fixed this for HTTP in 2011. Nothing has fixed it for AI-driven feature work.

Today I'm changing that.

**Introducing AgentSpec** — a small, opinionated spec format that AI coding agents (Claude Code, Cursor, Aider, Cline) can actually consume reliably.

Three things make it different from a markdown ticket:

**· Required fields for what English always skips** — enumerated error modes (not "handle errors properly"), measurable acceptance criteria (not "works correctly"), and an explicit `out_of_scope` that tells the agent exactly where to stop.

**· A linter that catches what wrecks agent output** — vague language ("fast", "secure", "improve" without thresholds), scope leakage ("etc.", "and so on"), untestable acceptance criteria, missing error modes. Every spec gets a 0–100 quality score before the agent ever sees it.

**· One spec → any agent** — compile to a Claude Code prompt brief, a Markdown brief for your reviewer, canonical YAML, or JSON. Same input. Whichever agent your team uses this quarter.

Zero build step. Runs entirely in your browser. Open DevTools → Network. Edit a spec, lint it, compile it. Watch the tab stay empty.

---

Live editor → https://umeshkvguptha.github.io/agentspec
Source → https://github.com/umeshkvguptha/agentspec
Open source, MIT.

---

Roadmap: Cursor and Aider compilers · VS Code extension with inline linting · GitHub Action that blocks PRs whose AgentSpec score drops · spec-to-PR provenance so every commit traces back to the spec hash that produced it · enterprise governance tier for regulated AI-SDLC — shared spec libraries, custom linter rules, audit trails for banking, brokerage, and healthcare. That last item is the reason this exists.

I built this while delivering a broker-dealer trading platform via agentic SDLC. After 20 years across FIS, Caesars Sportsbook, and Wissen, this is the layer I needed and couldn't buy. So I built it.

If you're running Claude Code or Cursor at any scale — open an issue with what you wish AgentSpec did.
If you want the enterprise governance tier when it ships — DM me.

#AgenticAI #ClaudeCode #Cursor #DeveloperTools #PromptEngineering #SpecDrivenDevelopment #OpenSource

---

## 5. Pre-baked answers for the comment section

**"Why not just OpenAPI?"**
OpenAPI describes HTTP surfaces. AgentSpec describes a *unit of feature work* — intent, out-of-scope, acceptance scenarios, invariants. They're orthogonal; an AgentSpec contract often *references* an OpenAPI fragment. Different layer entirely.

**"Why not just a detailed markdown ticket?"**
Markdown is what AgentSpec compiles *to* for human reading. The point of AgentSpec is the required-field schema and the linter that fires on the patterns markdown lets through silently.

**"Won't models improve and make this unnecessary?"**
Models have improved 10× in 18 months and the spec problem has gotten *worse*, not better — because we now ship more agentic code per spec. Better agents make inputs more leveraged, not less. A sharper chisel demands a steadier hand.

**"Is this a startup or a side project?"**
The browser editor and linter are free forever, MIT. The enterprise tier — shared spec libraries, custom linter rules, audit trail, on-prem — is what I'm building toward. DM me if you'd buy that today.

---

## 6. First-week follow-up posts

### Day 3 — The linter demo (image post)
> Before/after: same spec, English vs. AgentSpec. Show the diff in agent output.
> Hook: "This is what a 40/100 AgentSpec looks like — and why the agent drifted."

### Day 7 — The `out_of_scope` field post
> Hook: "The most underused field in software specs. AI agents pay the price."
> Explain why an explicit fence is the highest-leverage thing you can add to any ticket.

### Day 14 — The enterprise angle
> Hook: "Banks can't audit vibes. They need to audit specs."
> Frame the enterprise governance tier for regulated-industry CTOs/engineering directors.
