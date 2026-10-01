# backend-14days

My 4-week journey from zero to building backends on my own by **vibe coding with Claude Code**, while learning to think like the engineer who is accountable for the result.

> I own the **what, why and when**. Claude Code handles the **how**. I verify everything before it ships.

📄 The full roadmap is in **[IMPLEMENTATION_PLAN.md](IMPLEMENTATION_PLAN.md)**.

---

## What's in this repo

| Path | What it is |
| --- | --- |
| `IMPLEMENTATION_PLAN.md` | The 4-week plan: projects, concepts, checkpoints |
| `CLAUDE.md` | Rules Claude Code follows in this repo *(created in P0)* |
| `LOG.md` | Three lines per session: what worked, what confused me, one open question *(starts in P0)* |
| `concepts/` | One concept card per idea (RAG, MCP, deadlocks, idempotency, …) *(fills up as I go)* |
| `projects/` | One folder per project, each with its own `SPEC.md` and README *(fills up as I go)* |

## Projects

| # | Project | What it teaches | Status |
| --- | --- | --- | --- |
| P0 | Warm-up | Toolchain, HTTP, REST, Git | ⬜ |
| P1 | Hackathon Starter Kit | Spec writing, architecture, auth, database, Docker, tests | ⬜ |
| P2 | Ticket Rush | Race conditions, locks, deadlocks, idempotency, rate limiting | ⬜ |
| P3 | Study Buddy | RAG over my lecture notes, embeddings, pgvector, streaming, evals | ⬜ |
| P4 | MCP Server | Exposing my backend as tools Claude can call | ⬜ |
| P5 | Automation | n8n workflows, webhooks, cron, event-driven design | ⬜ |
| P6 | Capstone | CI/CD, observability, deployment, security review | ⬜ |
| — | Solo Hackathon Test | Blank repo → deployed app in ~7 hours | ⬜ |

---

## How I work with AI

Vibe coding is the default: Claude Code writes most of the code. The rules below keep me in the architect's seat so I end up with backends I understand and can debug.

### 1. Spec before code
Every project and feature starts with a `SPEC.md` that I write myself: goal, users, requirements, constraints, non-goals and a data model sketch. Claude may critique it, but it doesn't write it.

### 2. Plan before build
Claude proposes an architecture and step plan in plan mode. I approve, edit or reject it before any code is written.

### 3. Small slices, always runnable
Claude implements one step at a time with tests. I run it, and I commit after every working slice.

### 4. Break it, then review it
After each feature, I attack it: concurrent requests, bad input, another user's data, a dead database or LLM. Then I ask Claude to review the diff against the seven lenses, and I decide what gets fixed.

### 5. If I can't explain it, it doesn't ship
For anything I can't explain line by line, I ask Claude to teach it, with a timeline or example, until I can. Concurrency, security and data modelling get this treatment every time.

### 6. Capture what I learned
Any new concept gets a card in `concepts/`, and every session ends with three lines in `LOG.md`.

---

## The seven lenses

Every project README answers these:

| Lens | Question |
| --- | --- |
| Requirements and constraints | What must it do, for whom, at what scale, cost and latency? What's out of scope? |
| Architecture | What are the components, how do they talk, and why this shape? |
| Restrictions | What must never happen, and how is it enforced? |
| Concurrency | What happens when two requests hit the same data at once? |
| Deadlocks and contention | Can anything wait on something that waits on it? Where are the locks and timeouts? |
| Robustness | What happens when the DB, LLM or an external API is slow, down or wrong? |
| Maintainability | Could someone change this safely in three months? |

---

## Prompts I reuse

```text
PLAN    Here is SPEC.md. Propose an architecture and a step-by-step build plan.
        List risks against the seven lenses. Don't write code yet.

BUILD   Implement step N only. Add tests. Tell me what to run to verify it.

REVIEW  Review this diff as a senior backend engineer: security, concurrency,
        error handling, maintainability. Rank issues by severity.

TEACH   Explain why this needs a transaction, with a timeline of two
        concurrent requests.

CARD    I just used X. Ask me 3 questions to check I understand it, then help
        me draft concepts/x.md.
```

## Red flags in AI-generated code

Check before every commit:

- [ ] SQL built with f-strings or string concatenation
- [ ] Update/delete without an ownership or permission check
- [ ] Read-then-write without a transaction or lock (race condition)
- [ ] Locks taken in inconsistent order, or no lock timeouts (deadlock risk)
- [ ] External calls (LLM, APIs) with no timeout or retry limit
- [ ] Secrets in code, stack traces in responses, unbounded inputs or page sizes
- [ ] LLM output trusted blindly (executed, or saved without validation)
- [ ] Code I can't explain line by line

---

## Session routine (~3.5 h)

| Block | Time | What |
| --- | --- | --- |
| Recall | 10 min | Explain yesterday's concept out loud, no notes |
| Spec / plan | 30 min | Update SPEC.md, run plan mode, decide |
| Vibe code | 1 h 45 min | Claude builds in small committed slices |
| Break and review | 45 min | Tests, attacks, Claude review, my fixes |
| Card and log | 10 min | Concept card, 3 lines in LOG.md, push |

## Stack

Python 3.12 · FastAPI · Pydantic v2 · SQLAlchemy 2.0 + Alembic · PostgreSQL (+ pgvector) · Redis · pytest + Locust · Docker · n8n · Claude Code · GitHub Actions · Render
