# Phase 1 Master Plan: Agentic Chain of Governance

## Purpose and Philosophy
This repository defines a domain-agnostic governance workflow where AI agents operate as constrained corporate functions. The objective is high delivery velocity without uncontrolled drift.

Core philosophy:
- File-based contracts are the law.
- Baton control (`ai/active_agent.txt`) prevents concurrent role confusion.
- Authorization lock (`ai/decision-lock.yaml`) prevents hallucinated approvals.
- Senior Judgmental Engineer drives 80/20 and 90/10 simplification.
- QA retry limits force escalation instead of infinite review loops.

## Required Artifacts and Why They Exist
- `ai/requirements.md`: single requirement baseline from which all work derives.
- `ai/constitution.yaml`: non-negotiable policies and forbidden actions.
- `ai/judgment.yaml`: engineering invariants, delivery mode, and infra ceiling.
- `ai/decision-lock.yaml`: explicit major decisions + user confirmation state.
- `ai/plan.md`: practical plan with milestones, acceptance, tests, and complexity budget.
- `ai/schema/schema.sql`: schema contract to stop ad hoc data model drift.
- `ai/review.md`: review verdict, violations, punch list, and retry count.
- `ai/ratify.yaml`: legal acceptance record (ratified vs not ratified).
- `ai/active_agent.txt`: hard baton; only one role acts at a time.
- `ai/iterations/ITER-0001.md`: append-only rationale trail for decisions.

## Agent Roles and Responsibilities
- Product Owner: requirements ownership; user-side confirmations.
- Senior Judgmental Engineer: simplification authority (80/20 cuts, defer complexity).
- Architect: decision lock ownership for major technical decisions.
- Planner: milestone and schema planning; no decision-lock edits.
- Dev/Builder: implementation only per approved milestone.
- Security Expert: threat-model-lite and risk blocking.
- Ops/SRE: operability and rollback readiness checks.
- QA/Reviewer-Ratifier: policy and contract enforcement; ratification and escalation.
- Judge/Mediator: deadlock arbitration after repeated failure.

## Manual Multi-Window Codex Runbook (Phase 1)

### 1) Setup
1. Populate `ai/requirements.md` from one-page requirement.
2. Confirm `ai/constitution.yaml` and `ai/judgment.yaml` defaults.
3. Initialize `ai/decision-lock.yaml` with unresolved questions and `confirmed_by_user: false`.
4. Ensure `ai/active_agent.txt` is `PRODUCT_OWNER`.
5. Ensure `ai/iterations/ITER-0001.md` exists and has decision/retry sections.

### 2) Iteration Cycle
1. **Product Owner intake** (`00-intake-product-owner.md`)
   - Updates requirements and answers architecture-changing questions.
2. **Senior Judgmental Engineer pass** (`01-senior-judgmental-engineer.md`)
   - Writes Simplification Plan, Deferred Complexity, and scaling exit criteria.
3. **Architect lock** (`02-architect.md`)
   - Finalizes stack/shape/tradeoffs in decision lock or loops back to Product Owner for unresolved questions.
4. **Planner contract creation** (`03-planner.md`)
   - Requires confirmed lock.
   - Creates milestone plan and schema diffs.
5. **Dev implementation** (`04-dev-builder.md`)
   - Implements Milestone 1 only.
6. **Security check** (`05-security-expert.md`)
   - PASS/FAIL/ESCALATE with citations.
7. **Ops/SRE check** (`06-ops-sre.md`)
   - PASS/FAIL/ESCALATE with citations.
8. **QA review and ratify** (`07-qa-reviewer-ratifier.md`)
   - Validates against constitution, judgment, decision lock, plan, and schema.
   - Updates ratification state.
9. **Judge/Mediator (if escalated)** (`08-judge-mediator.md`)
   - Resolves two-fail loops and conflicts.

### 3) Stop Conditions and Enforcement
- **Missing baton**: role must output `WAITING FOR BATON` and stop.
- **Missing confirmation** (`confirmed_by_user != true`): Planner/Dev/Security/Ops/QA must output `WAITING FOR USER` and stop.
- **Schema changes needed but absent from plan**: return baton to Planner.
- **Two FAILs for same milestone**: QA sets `ESCALATE`; baton goes to Judge, not Dev.

### 4) Retry and Escalation Rule
- `failure_count_for_milestone` is tracked in `ai/review.md`.
- First FAIL: baton returns to Dev for fixes.
- Second FAIL (same milestone): mandatory escalation to Judge/Mediator.
- Judge may split milestone, alter lock with justification, add ADR, or route back to Architect/Planner/Dev.

## Definition of Done for Phase 1
Phase 1 is done when all are true:
- At least 2-3 iterations completed with artifacts maintained each cycle.
- At least one escalation occurred and was resolved through Judge/Mediator.
- Decision lock confirmation gate was respected.
- Schema ownership stayed Planner-controlled.
- Ratification records were produced per completed milestone.

## Operating Notes
- Keep each milestone small enough to review in one pass.
- Prefer single deployable and containers-only unless policy explicitly upgrades.
- Treat non-goals in `ai/judgment.yaml` as hard boundaries in MVP mode.
