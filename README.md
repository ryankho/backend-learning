# AI Usage Rules

> How I use AI while learning backend development in this repo.
> Goal: I am the **architect and reviewer**. AI is the typist.

Juniors who let AI drive end up with backends they cannot debug. These rules exist so that by Day 14 I can build a backend on my own, with AI making me faster rather than replacing my understanding.

---

## The 5 rules

### 1. Morning = no AI code generation
Learn the day's concept by reading the official docs and typing the examples myself.
AI may only **explain** (e.g. *"Why does this return 422?"*). It never writes the code.

### 2. Afternoon = AI as pair-programmer
Describe the design first (endpoint, schema, expected behaviour), then let AI draft.
**I must be able to explain every line before I commit it.** If I can't, I ask AI to explain it or I rewrite it myself.

### 3. Spec before prompt
Before prompting, write a 5-line spec as a comment:

```python
# Spec: POST /tasks
# Input:  title (1-200 chars), project_id, optional due_date
# Output: 201 + created task
# Errors: 401 not logged in, 404 project missing or not mine, 422 bad input
# Edge:   due_date in the past is rejected
```

Paste that spec as the prompt. Vague prompts produce plausible-looking wrong code.

### 4. Verify, don't trust
Run it, test it, then ask AI: *"What are the security problems in this code?"*
Check every AI-generated change against the checklist below.

### 5. Break it on purpose
Once a day, delete or change something and **predict the error before running it**.
This builds the debugging instinct that AI can't give me.

---

## Prompt template

```text
I'm building <endpoint/feature> in FastAPI with SQLAlchemy 2.0 and PostgreSQL.

Schema: <models / fields involved>
Rules:  <who is allowed to do what>
Errors: <status codes for each failure case>

Write the route and a pytest for <the most important failure case>.
Explain any design choice you make.
```

**Example:**
> I'm building `POST /tasks` in FastAPI with SQLAlchemy 2.0. Schema: … Rules: only the owner can create tasks in their project; return 404 if the project doesn't exist or isn't theirs. Write the route and a pytest for the forbidden case. Explain any design choice you make.

---

## Checklist for AI-generated code

Common mistakes AI makes in backends. Check each one before committing:

- [ ] **SQL injection**: no SQL built with f-strings or string concatenation
- [ ] **Missing auth checks**: update/delete endpoints verify the current user owns the resource
- [ ] **Hard-coded secrets**: keys, passwords and DB URLs come from environment variables
- [ ] **N+1 queries**: relationships loaded with `selectinload` / `joinedload` where needed
- [ ] **No input limits**: string lengths, page sizes and file sizes are bounded
- [ ] **Leaky errors**: responses don't expose stack traces or internal details
- [ ] **Tests exist**: at least one happy-path test and one failure-case test
- [ ] **I understand it**: I can explain every line without looking at the AI's explanation

---

## Daily AI workflow

| Block    | Time   | AI allowed?                     |
| -------- | ------ | ------------------------------- |
| Warm-up  | 15 min | No                              |
| Learn    | 2 h    | Explain only, no code gen       |
| Build    | 3 h    | Yes, as pair-programmer         |
| Break-it | 30 min | No, predict errors myself       |
| Review   | 30 min | Yes, AI reviews my diff; I fix  |

End each day with 3 lines in `LOG.md`: what worked, what confused me, and one question for tomorrow.
