# Multi-Agent Codex Governance Template

## What this repository is
This repository is a **governance template** for building software with coordinated Codex agents using a file-based handoff process.

It gives you:
- A shared workflow for role-based execution (Product Owner, Architect, Dev, QA, etc.)
- A durable handoff mechanism in `ai/next_agent.yaml`
- Checkpoints and review artifacts so progress is visible between runs

It is **not**:
- A finished product scaffold with opinionated app code
- A replacement for your product requirements or engineering judgment
- A guarantee of correctness without human review

## Core idea / mental model
Think of this template like a relay race:
1. Each role runs in sequence.
2. The current role writes decisions and outputs to files under `ai/`.
3. The next role is defined in `ai/next_agent.yaml`.
4. You run the next Codex session by following that handoff exactly.

The key principle is: **state lives in files, not in chat memory**.

## Repository layout
Use this as a practical convention:

```text
.
├─ ai/          # governance, prompts, plans, handoff files, reviews
├─ apps/        # application code (web, api, worker, etc.)
└─ infra/       # deployment/infrastructure code (IaC, env, ops config)
```

Notes:
- `ai/` is the process backbone.
- `apps/` and `infra/` are where implementation should live.
- Keep changes traceable through `ai/` artifacts.

## How to use this template (step-by-step)

### Step 0: Clone or fork
Clone this repository (or fork it first), then work on your own branch/repo as usual.

```bash
git clone <your-fork-or-repo-url>
cd <repo>
```

### Step 1: Write requirements in `ai/requirements.md`
Capture your goals, constraints, scope, and success criteria in `ai/requirements.md` before starting agents.

### Step 2: Start with the Product Owner prompt
Begin with the Product Owner stage using:
- `ai/prompts/00-intake-product-owner.md`

In your Codex run, provide that prompt and your `ai/requirements.md` context.

## Codex session hygiene (context bleed)
When switching roles, avoid carrying stale context between runs.

Recommended practice:
- Use separate windows/tabs per role, **or**
- Disconnect and start a fresh Codex session for each new role

This reduces “context bleed” where one role’s assumptions leak into the next role’s decision-making.

## How to continue (no prompt pasting)
After the first run, do **not** manually paste long role prompts each time.

Use the handoff file:
- `ai/next_agent.yaml`

In the next Codex session, type exactly:

```text
Follow ai/next_agent.yaml exactly.
```

Important:
- Do **not** just type “continue”.
- Do **not** add your own summary of extra instructions unless you intentionally want to override the handoff.

## If something feels wrong (how to debug)
If the process stalls, loops, or outputs look inconsistent, check:
- `ai/review.md` (latest critique/findings)
- `ai/iterations/ITER-0001.md` (iteration state and history)
- `ai/active_agent.txt` (who is currently expected to act)

These files usually reveal where flow control or expectations broke.

## Typical flow
A common sequence is:

1. Product Owner
2. SJE (Senior Judgmental Engineer)
3. Architect
4. Planner
5. Dev
6. Security
7. Ops
8. QA

Then loop as needed based on review outcomes.

## What this template optimizes for
This template is best for:
- Small to medium applications
- A single primary deployable (or tightly related components)
- Teams that value clear handoffs, auditability, and practical iteration speed

## Final note
If you find gaps, rough edges, or role conflicts, improve the template and commit those improvements. Treat the process itself as a product that should evolve with your team.
