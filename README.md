# Token Efficiency & Context Continuity Protocol (TECCP)

> A Claude skill for maximizing output quality while minimizing token expenditure, with seamless multi-session project continuity via GitHub-backed state persistence.

## What This Solves

When using Claude (especially Opus 4.5/4.6), three problems consistently arise:

1. **Token Burn** — The model generates verbose, over-explained responses that exhaust the context window rapidly
2. **Session Death** — When the context window fills, all project state and decisions are lost
3. **Hallucination Drift** — Across sessions, the model may fabricate or contradict prior decisions without a verified source of truth

TECCP addresses all three through a structured protocol that operates as a Claude skill.

## How It Works

### Three Pillars

| Pillar | What It Does |
|---|---|
| **Token Efficiency Engine** | Enforces radical concision rules — eliminates filler, scales responses to complexity, uses progressive disclosure |
| **Context Buffer System** | Monitors context depletion, proactively alerts before capacity, generates structured handoff documents |
| **GitHub Task Tracking** | Persists task registry, architectural decisions, and verified facts to GitHub — survives any session boundary |

## Repository Structure

When TECCP is initialized for a project, it creates:

```
your-project/
├── README.md
├── .teccp/
│   ├── task-registry.md         # Master task list (source of truth)
│   ├── sessions/
│   │   ├── session-001.md       # Context Continuity Documents
│   │   └── ...
│   ├── decisions.md             # Architectural Decision Record
│   └── anti-hallucination.md    # Verified facts registry
├── src/                         # Your project files
└── docs/
```

## User Commands

| Command | Action |
|---|---|
| `status` | Show task registry summary |
| `ccd` | Generate Context Continuity Document now |
| `push` | Push current state to GitHub |
| `validate` | Run anti-hallucination check |
| `buffer` | Check estimated context capacity |
| `next` | Show next pending task |
| `scope` | Show scope summary + drift warnings |

## Installation

### As a Claude Project Skill

1. Copy `SKILL.md` to your Claude project's knowledge base
2. The skill auto-triggers on relevant keywords

### As a Standalone Prompt

Paste the contents of `SKILL.md` at the start of a new conversation, or reference it from a GitHub-connected MCP session.

## License

MIT
