---
name: token-efficiency-context-continuity
description: "Use this skill for ANY multi-session project or complex task where token conservation, context window longevity, and seamless session handoff are critical. Triggers include: any mention of 'token efficiency', 'context continuity', 'session handoff', 'pick up where we left off', 'continue from last session', 'save progress', 'task tracking', long-running projects, multi-chat workflows, or when the user wants to maximize output quality while minimizing token expenditure. Also triggers when building anything that spans multiple chat sessions, when the user mentions running out of context, or when working on projects that require GitHub-backed state persistence. ALWAYS use this skill when the user references efficiency protocols, context buffers, or task-based project management across sessions."
---

# Token Efficiency & Context Continuity Protocol (TECCP)

## Purpose

This skill ensures maximum value extraction per token while maintaining perfect project continuity across chat sessions. It operates on three pillars:

1. **Token Discipline** — Radical concision without sacrificing quality
2. **Context Buffer System** — Proactive session-end detection and state serialization
3. **GitHub-Backed Task Tracking** — Persistent, versioned project state that eliminates duplication and hallucination

---

## PHASE 1: TOKEN EFFICIENCY ENGINE

### 1.1 Response Architecture Rules

Follow these rules for EVERY response in the session:

**ELIMINATE immediately:**
- Preambles ("Certainly!", "Great question!", "I'd be happy to help")
- Postambles ("Let me know if you need anything else", "Hope this helps!")
- Restating the user's question back to them
- Hedging language unless genuine uncertainty exists
- Meta-commentary about your own process (unless explicitly useful)
- Redundant transitions between sections
- Filler adjectives and adverbs that don't add information

**STRUCTURE over prose:**
- Default to structured formats (bullets, tables, code blocks) for information-dense responses
- Use prose only when narrative flow genuinely serves comprehension
- Nested bullets for hierarchical information
- Headers only when segmenting genuinely distinct sections

**DENSITY targets:**
- One concept per bullet point, maximum information per sentence
- Technical shorthand when the user's expertise level permits it
- Combine related points rather than separating them
- Aim for 30% fewer tokens than your natural output length

**PROGRESSIVE DISCLOSURE:**
- Core answer first, always
- Signal availability of detail: "Can expand on X if needed" (costs 6 tokens vs. 200+ for unsolicited detail)
- Never auto-elaborate unless the task explicitly requires comprehensive coverage

### 1.2 Thinking Block Discipline

When using extended thinking (if available):
- Keep thinking focused and conclusive — no circular reasoning
- Think only when genuinely uncertain or when complex multi-step reasoning is required
- Exit thinking as soon as a conclusion is reached
- Never rehearse the response in thinking — go straight to output

### 1.3 Response Scaling Matrix

| Query Complexity | Target Response Length | Format |
|---|---|---|
| Simple factual | 1-3 sentences | Inline prose |
| Clarification needed | 1 question + brief context | Prose |
| Moderate task | 5-15 lines | Structured bullets |
| Complex task | 20-50 lines | Headers + bullets/code |
| Full implementation | Modular chunks, request permission to continue | Files + minimal commentary |

### 1.4 Code Efficiency

When generating code:
- Inline comments only for non-obvious logic
- No boilerplate explanations before/after code blocks unless the user is learning
- If code exceeds 50 lines, create a file rather than displaying inline
- Reference existing code rather than repeating it

---

## PHASE 2: CONTEXT BUFFER SYSTEM (CBS)

### 2.1 Context Window Monitoring

Claude cannot directly measure remaining context, but can infer depletion through these signals:

**Depletion Indicators (monitor continuously):**
- Conversation has exceeded ~15 substantial back-and-forth exchanges
- Multiple large code blocks or documents have been generated
- User has pasted large inputs (files, logs, data)
- Response quality begins to degrade (repetition, missed details)
- Total estimated session tokens approaching 150K+ range

**Proactive Warning Triggers:**
When ANY two of the above indicators are true, Claude MUST:
1. Alert the user: `⚠️ CONTEXT BUFFER: Session approaching capacity. Recommend generating handoff document now.`
2. Offer to generate the Context Continuity Document (CCD) immediately
3. Continue working if user declines, but re-alert after 3 more exchanges

### 2.2 Context Continuity Document (CCD)

When triggered (by user request OR buffer warning), generate a CCD with this exact structure:

```markdown
# Context Continuity Document (CCD)
## Generated: [YYYY-MM-DD HH:MM UTC]
## Project: [Project Name]
## Session: [Sequential session number]

---

### 🎯 PROJECT OBJECTIVE
[1-2 sentence project goal]

### 📋 TASK REGISTRY

#### ✅ COMPLETED
- [TASK-001] [Description] — Completed [date]
  - Output: [file/artifact reference]
  - Key decisions: [any non-obvious choices made]
- [TASK-002] ...

#### 🔄 IN PROGRESS
- [TASK-003] [Description] — Started [date]
  - Current state: [precise description of where work stopped]
  - Next action: [exact next step to take]
  - Blockers: [any dependencies or issues]

#### 📌 PENDING
- [TASK-004] [Description] — Priority: [HIGH/MEDIUM/LOW]
  - Dependencies: [TASK-XXX]
  - Estimated scope: [brief]
- [TASK-005] ...

#### ❌ CANCELLED / DESCOPED
- [TASK-006] [Description] — Reason: [why removed]

### 🏗️ ARCHITECTURE & KEY DECISIONS
[Bullet list of architectural choices, tech stack decisions, design patterns adopted — anything a new session MUST know to avoid contradicting prior work]

### 📁 FILE MANIFEST
[List of all files created/modified with paths and brief descriptions]

### ⚠️ KNOWN ISSUES & WARNINGS
[Any bugs, edge cases, or concerns flagged during the session]

### 🔗 DEPENDENCIES & EXTERNAL REFERENCES
[APIs, libraries, external docs, repo URLs referenced]

### 💡 SESSION INSIGHTS
[Any non-obvious learnings, user preferences, or patterns observed that should persist]

### 📊 PROGRESS SUMMARY
- Total tasks: [N]
- Completed: [N] ([%])
- In progress: [N]
- Pending: [N]
- Cancelled: [N]

### 🚀 NEXT SESSION ENTRY POINT
[Exact instruction for the next session to begin with, e.g.:]
"Read CCD from [GitHub path]. Resume from TASK-003: [specific next action]. Validate no regression on completed tasks before proceeding."
```

### 2.3 CCD Quality Gates

Before finalizing a CCD, validate:
- [ ] Every task has a unique ID (TASK-NNN format)
- [ ] No duplicate tasks exist
- [ ] All completed tasks reference their output
- [ ] In-progress tasks have precise "current state" and "next action"
- [ ] Architecture decisions are captured (prevents contradictory choices in next session)
- [ ] File manifest is complete and accurate
- [ ] Next session entry point is specific and actionable

---

## PHASE 3: GITHUB-BACKED TASK TRACKING

### 3.1 Repository Structure

All project state lives in a GitHub repo with this structure:

```
project-name/
├── README.md                    # Project overview + quick-start for new sessions
├── .teccp/                      # TECCP system files
│   ├── task-registry.md         # Master task list (single source of truth)
│   ├── sessions/
│   │   ├── session-001.md       # CCD from session 1
│   │   ├── session-002.md       # CCD from session 2
│   │   └── ...
│   ├── decisions.md             # Architectural Decision Record (ADR)
│   └── anti-hallucination.md    # Facts registry (see Phase 4)
├── src/                         # Project source code
├── docs/                        # Project documentation
└── ...                          # Other project files
```

### 3.2 Task Registry Format (`.teccp/task-registry.md`)

```markdown
# Task Registry — [Project Name]
## Last Updated: [YYYY-MM-DD HH:MM UTC]
## Current Session: [N]

### Legend
- ✅ Complete | 🔄 In Progress | 📌 Pending | ❌ Cancelled | 🔒 Blocked

---

| ID | Task | Status | Priority | Dependencies | Session | Notes |
|---|---|---|---|---|---|---|
| TASK-001 | [Description] | ✅ | HIGH | — | S001 | [Output ref] |
| TASK-002 | [Description] | 🔄 | HIGH | TASK-001 | S002 | [Current state] |
| TASK-003 | [Description] | 📌 | MEDIUM | TASK-002 | — | — |
| TASK-004 | [Description] | 🔒 | LOW | TASK-003 | — | Blocked by [X] |

---

### Change Log
- [YYYY-MM-DD] Session N: Added TASK-005, completed TASK-001, TASK-002
- [YYYY-MM-DD] Session N-1: Initial task decomposition
```

### 3.3 GitHub Operations Protocol

**At Session Start:**
1. Fetch `task-registry.md` from GitHub
2. Fetch the latest CCD from `.teccp/sessions/`
3. Fetch `decisions.md` and `anti-hallucination.md`
4. Validate: cross-reference task registry against CCD for consistency
5. Present summary to user: "Resuming from Session N. [X] tasks complete, [Y] in progress, [Z] pending. Next action: [specific task]."
6. **Ask user to confirm** before proceeding — never auto-resume without validation

**During Session:**
- Update task statuses as work completes
- Commit significant milestones to GitHub (don't wait until session end)
- Push code/files as they're finalized

**At Session End (or Buffer Warning):**
1. Generate CCD
2. Update task-registry.md
3. Push both to GitHub in a single commit
4. Confirm push success to user

### 3.4 Commit Message Convention

```
[TECCP-SN] Brief description

Session: N
Tasks completed: TASK-XXX, TASK-YYY
Tasks started: TASK-ZZZ
Next: [brief next action]
```

---

## PHASE 4: ANTI-HALLUCINATION FRAMEWORK

### 4.1 Facts Registry (`.teccp/anti-hallucination.md`)

Maintain a living document of verified facts about the project:

```markdown
# Facts Registry — [Project Name]

## Verified Facts (NEVER contradict these)
- Tech stack: [exact versions]
- API endpoints: [exact URLs]
- Data models: [exact schemas]
- Business rules: [exact logic]
- User preferences: [stated preferences]

## Assumptions (flagged, not verified)
- [Assumption] — Flagged Session N, awaiting confirmation

## Corrections (things that were wrong and fixed)
- [Original claim] → [Corrected to] — Session N
```

### 4.2 Anti-Hallucination Behaviors

**ALWAYS:**
- Cite the source when referencing prior decisions (e.g., "Per TASK-003 / Session 2")
- Flag uncertainty explicitly: "I believe X based on [source], but confirm?"
- Cross-reference against facts registry before stating project-specific facts
- When generating code, verify imports/dependencies exist before referencing them
- When referencing files, verify they exist in the file manifest

**NEVER:**
- Invent API endpoints, function names, or file paths not in the manifest
- Assume a library version without checking
- Claim a task is complete without evidence
- Fabricate user requirements not explicitly stated
- Guess at architectural decisions — check `decisions.md` first

### 4.3 Validation Checkpoints

At these moments, pause and validate:

| Checkpoint | Validation Action |
|---|---|
| Session start | Cross-check task registry vs. latest CCD |
| Before coding | Verify tech stack and dependencies in facts registry |
| After completing a task | Confirm output matches task acceptance criteria |
| Before session end | Ensure all work is captured in CCD + pushed to GitHub |
| On any uncertainty | Flag it, don't fabricate an answer |

---

## PHASE 5: SESSION LIFECYCLE

### 5.1 New Session Opening Script

When a user starts a new session referencing a TECCP-managed project:

```
1. FETCH: Pull task-registry.md, latest CCD, decisions.md, anti-hallucination.md from GitHub
2. VALIDATE: Cross-check for consistency. Flag any discrepancies.
3. PRESENT: 
   "📋 Session [N+1] — [Project Name]
    Progress: [X/Total] tasks complete ([%])
    Last session ended at: [TASK-ID] — [description]
    Resuming with: [next task and specific action]
    
    ⚠️ Review queue: [any flagged items needing confirmation]
    
    Ready to proceed?"
4. WAIT for user confirmation before executing any work
5. APPLY efficiency protocol for all responses in this session
```

### 5.2 Mid-Session Health Check

Every ~8-10 exchanges, silently evaluate:
- Am I still following token efficiency rules?
- Have I introduced any information not grounded in verified sources?
- Should I push a checkpoint commit to GitHub?
- Is the context buffer approaching capacity?

If any concern is found, surface it concisely to the user.

### 5.3 Session Closing Script

When ending a session (by user request or buffer warning):

```
1. GENERATE CCD with all quality gates passed
2. UPDATE task-registry.md with current state
3. UPDATE anti-hallucination.md if new facts emerged
4. UPDATE decisions.md if new architectural choices were made
5. PUSH all updates to GitHub in a single commit
6. PRESENT summary:
   "✅ Session [N] closed.
    Completed: [list]
    In progress: [list with state]
    Pushed to: [repo/branch]
    Next session starts with: [exact instruction]"
```

---

## PHASE 6: ENHANCED SAFEGUARDS & OPTIMIZATIONS

### 6.1 Duplicate Prevention Protocol

Before creating ANY new task:
1. Search existing task registry for similar descriptions
2. Check completed tasks — the work may already be done
3. Check cancelled tasks — there may be a reason it was descoped
4. If potential overlap found: "This resembles TASK-XXX ([status]). Create new, or update existing?"

### 6.2 Scope Drift Detection

Monitor for scope creep by tracking:
- Tasks added mid-session that weren't in the original plan
- Feature requests that expand beyond the stated project objective
- When detected: "⚡ Scope note: This adds [X] which wasn't in the original plan. Add to task registry as new scope, or defer?"

### 6.3 Progressive Context Loading

For very large projects, don't load everything at session start:
1. Always load: `task-registry.md` (lightweight, master index)
2. Load on demand: Latest CCD (only if resuming mid-task)
3. Load on demand: `decisions.md` (only when making architectural choices)
4. Load on demand: `anti-hallucination.md` (only when stating project facts)
5. Never load: Old session CCDs (reference only if specific historical context needed)

This preserves context window for actual work.

### 6.4 Emergency Recovery

If a session ends unexpectedly (user disconnects, error, etc.) and the CCD wasn't generated:
- Next session: Check GitHub for latest state
- Cross-reference with conversation history (if available through past chats tools)
- Reconstruct state from available evidence
- Flag any gaps: "⚠️ Recovery mode: Last session may not have been fully captured. Please verify these items..."

### 6.5 Multi-Branch Support

For complex projects with parallel workstreams:
- Use Git branches to isolate workstreams
- Each branch gets its own task subset in the registry
- CCD includes branch context
- Merge protocol: Update task registry on main after branch merge

---

## QUICK REFERENCE CARD

### Token Efficiency Shortcuts
| Instead of... | Say... |
|---|---|
| "I'd be happy to help with that!" | [just start helping] |
| "Let me explain how this works..." | [just explain it] |
| "Here's what I think we should do:" | [just state the plan] |
| 3 paragraphs of context | 3 bullet points |
| Repeating the question | → answer directly |

### Session Commands (user can invoke these)
| Command | Action |
|---|---|
| `status` | Show task registry summary |
| `ccd` | Generate Context Continuity Document now |
| `push` | Push current state to GitHub |
| `validate` | Run anti-hallucination check on recent work |
| `buffer` | Check estimated context capacity |
| `next` | Show next pending task with full context |
| `scope` | Show scope summary and any drift warnings |

---

## INITIALIZATION

When this skill is first activated for a project:

1. **Ask** the user for: project name, GitHub repo (existing or create new), and project objective
2. **Create** the `.teccp/` directory structure in the repo
3. **Decompose** the project objective into an initial task list with the user
4. **Push** the initial task-registry.md, decisions.md, and anti-hallucination.md
5. **Confirm** setup: "TECCP initialized. [N] tasks in registry. Token efficiency active. Context buffer monitoring active."
6. **Begin work** on the first task
