# AI Coding Startkit

A collection of **Genesis Protocols** — structured bootstrap prompts for AI coding agents. Paste one into your agent at the start of a project and it will interview you, scaffold a compliant project environment, and set up the rules, commands, and workflows needed for long-running autonomous development with built-in drift resistance.

Two families are included: one for **Claude Code** (Anthropic) and one for **Antigravity** (Google).

---

## What is a Genesis Protocol?

A Genesis Protocol is a prompt you give to an AI coding agent before any code exists. It instructs the agent to act as a Solutions Architect and walk through a structured setup sequence:

1. **Discovery** — The agent interviews you about what you're building, your stack, and your quality requirements.
2. **Scaffolding** — Creates the directory structure (`src/`, `tests/`, `docs/`, agent config directory).
3. **Core documents** — Generates four living documents that form a *circularity loop*:
   - `README.md` — project description, success criteria, quick start
   - `PLAN.md` — living task ledger with objectives, backlog, and key insights
   - `docs/ARCHITECTURE.md` — stack, module boundaries, testing pyramid, required verification commands
   - `docs/DECISIONS.md` — decision log seeded with the architecture choice from discovery
4. **Rules / instructions** — Creates the agent's behavioral contract (planning gate, architecture lock, verification mandate, drift detection, git discipline, failure escalation).
5. **Skills / commands procurement** — Searches curated community repositories for existing high-quality skills before generating new ones. Falls back to generating internal skills for gaps.
6. **Workflows** — Installs `feature`, `repair`, and `bootstrap-stack` workflows.
7. **Handoff validation** — Checks every required file exists, then asks: *"Environment ready. What is the first feature or vertical slice?"*

---

## The Circularity Loop

The four core documents form a closed system. Every code change must be traceable to `PLAN.md`. Every architectural change must be recorded in `docs/DECISIONS.md` and reflected in `docs/ARCHITECTURE.md`. Every user-facing change must update `README.md`. No document may contradict another.

This circularity is the primary mechanism for preventing drift across long sessions and agent handovers.

---

## Protocols

### Claude Code Genesis Protocol

For use with [Claude Code](https://claude.ai/code) (Anthropic).

| File | Version | Notes |
|------|---------|-------|
| [Claude Code Genesis Protocol v1.1.md](Claude%20Code%20Genesis%20Protocol%20v1.1.md) | v1.1 | Current |
| [Claude Code Genesis Protocol v1.0.md](Claude%20Code%20Genesis%20Protocol%20v1.0.md) | v1.0 | |

**Agent config directory:** `.claude/`
**Rules:** `CLAUDE.md` (loaded automatically every session)
**Commands:** `.claude/commands/<name>.md` (invoked as `/<name>`)
**Settings / hooks:** `.claude/settings.json`

### Antigravity Genesis Protocol

For use with Antigravity (Google).

| File | Version | Notes |
|------|---------|-------|
| [ANTIGRAVITY GENESIS PROTOCOL v4.5.md](ANTIGRAVITY%20GENESIS%20PROTOCOL%20v4.5.md) | v4.5 | Current |
| [ANTIGRAVITY GENESIS PROTOCOL v4.0.md](ANTIGRAVITY%20GENESIS%20PROTOCOL%20v4.0.md) | v4.0 | |
| [ANTIGRAVITY GENESIS PROTOCOL v3.1.md](ANTIGRAVITY%20GENESIS%20PROTOCOL%20v3.1.md) | v3.1 | |


**Agent config directory:** `.agent/`
**Rules:** `.agent/rules/*.md`
**Skills:** `.agent/skills/<name>/SKILL.md`
**Workflows:** `.agent/workflows/<name>.md`

---

## How to Use

1. Open a new project directory (empty, or an existing repo with no agent config yet).
2. Open your AI coding agent (Claude Code, Antigravity, etc.).
3. Copy the contents of the appropriate protocol file.
4. Paste it as your first message.
5. Answer the discovery interview. The agent will ask about what you're building, your stack, and your quality bar.
6. The agent scaffolds the environment, procures skills, and confirms every required file exists.
7. Start building — use `/feature <name>` (Claude Code) or the `feature` workflow (Antigravity) for all subsequent work.

Use the most recent version of the protocol for your agent unless you have a specific reason to use an older one.

---

## Key Concepts

**Planning gate** — No code is modified without an explicit implementation plan (goal, files to change, steps, risks, verification commands).

**Architecture lock** — After `docs/ARCHITECTURE.md` is created, no new frameworks or patterns are introduced without a recorded decision.

**Verification mandate** — After any change, the required verification suite must pass before new work begins.

**Drift detection** — The agent continuously checks that implementation, plan, architecture docs, and README all agree. Contradictions are resolved immediately.

**Failure escalation** — If verification fails more than twice on the same issue, the agent stops, writes a failure analysis, revises the plan, then retries. After five total failures, scope is reduced and the issue is logged.

**Skill / command governance** — External skills and commands are helpers, not authorities. Local rules always win. Low-quality external skills are logged and avoided.
