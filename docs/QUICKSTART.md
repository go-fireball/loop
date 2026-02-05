# Quickstart: Run ITER-0001 in ~20 minutes (Manual Multi-Window)

Use this as a copy/paste operational runbook for your first full baton-driven pass.

## 1) Prereqs checklist (2 minutes)

- [ ] Repo is clean (`git status` shows no unrelated edits).
- [ ] `ai/prompts/00-intake-product-owner.md` through `ai/prompts/08-judge-mediator.md` exist.
- [ ] Core control files exist: `ai/active_agent.txt`, `ai/decision-lock.yaml`, `ai/plan.md`, `ai/review.md`, `ai/ratify.yaml`.
- [ ] Required working files exist: `ai/requirements.md`, `ai/simplification.md`, `ai/schema/schema.sql`, `ai/iterations/ITER-0001.md`.
- [ ] `ai/active_agent.txt` is set to `PRODUCT_OWNER` before starting.

## 2) Starter content for `ai/requirements.md` (paste-ready)

Paste and adjust:

```md
# Requirements (Starter)

- Build a small internal web tool for tracking team tasks.
- Users can create, edit, complete, and delete tasks.
- Include basic filtering (all, active, completed).
- Persist data in a relational database.
- Keep deployment simple: single deployable service.
- Add basic audit fields (created_at, updated_at) on task records.
- Require lightweight auth for internal users only.
```

## 3) Default baton order for Phase 1

`PRODUCT_OWNER -> SENIOR_JUDGMENTAL_ENGINEER -> ARCHITECT -> PLANNER -> DEV_BUILDER -> SECURITY_EXPERT -> OPS_SRE -> QA_REVIEWER_RATIFIER -> (next milestone loops via PLANNER/DEV_BUILDER, or escalates to JUDGE_MEDIATOR when needed)`

## 4) First-iteration runbook (10-15 minutes)

For each role turn:
1. Set `ai/active_agent.txt` to that role (or confirm it was handed off correctly).
2. Open a Codex window/session for that role.
3. Paste/use the matching prompt file from `ai/prompts/`.
4. Accept and save only allowed file edits.
5. Confirm role printed `FINISHED: HANDING TO <NEXT_ROLE>`.

### Step-by-step for ITER-0001

1. **PRODUCT_OWNER** (`ai/prompts/00-intake-product-owner.md`)
   - Should update requirements/answers only.
   - Expected file changes: `ai/requirements.md`, maybe `ai/decision-lock.yaml` (question responses), `ai/iterations/ITER-0001.md`, `ai/active_agent.txt`.

2. **SENIOR_JUDGMENTAL_ENGINEER** (`ai/prompts/01-senior-judgmental-engineer.md`)
   - Makes 80/20 simplification calls.
   - Expected file changes: `ai/simplification.md`, `ai/iterations/ITER-0001.md`.

3. **ARCHITECT** (`ai/prompts/02-architect.md`)
   - Locks major technical decisions.
   - Expected file changes: `ai/decision-lock.yaml`, `ai/iterations/ITER-0001.md`, `ai/active_agent.txt`.
   - If open questions remain, baton returns to PRODUCT_OWNER first.

4. **PLANNER** (`ai/prompts/03-planner.md`)
   - Produces milestones + schema plan.
   - Expected file changes: `ai/plan.md`, `ai/schema/schema.sql`, `ai/iterations/ITER-0001.md`, `ai/active_agent.txt`.

5. **DEV_BUILDER** (`ai/prompts/04-dev-builder.md`)
   - Implements Milestone 1 only.
   - Expected file changes: product code/tests for milestone, `ai/iterations/ITER-0001.md`, `ai/active_agent.txt`.

6. **SECURITY_EXPERT** (`ai/prompts/05-security-expert.md`)
   - Performs security review verdict.
   - Expected file changes: `ai/review.md`, `ai/iterations/ITER-0001.md`, `ai/active_agent.txt`.

7. **OPS_SRE** (`ai/prompts/06-ops-sre.md`)
   - Performs operability/reliability review verdict.
   - Expected file changes: `ai/review.md`, `ai/iterations/ITER-0001.md`, `ai/active_agent.txt`.

8. **QA_REVIEWER_RATIFIER** (`ai/prompts/07-qa-reviewer-ratifier.md`)
   - Performs final milestone verdict + ratification.
   - Expected file changes: `ai/review.md`, `ai/ratify.yaml`, `ai/iterations/ITER-0001.md`, `ai/active_agent.txt`.

## 5) `WAITING FOR BATON` means

Meaning: wrong role tried to act.

Fix:
- Set `ai/active_agent.txt` to the intended role.
- Re-run that role with its own prompt.
- Ensure prior role ended with `FINISHED: HANDING TO <NEXT_ROLE>`.

## 6) `WAITING FOR USER` means

Meaning: execution is blocked until user confirmation is recorded.

Fix:
- Answer open questions in `ai/decision-lock.yaml`.
- Set `confirmed_by_user: true` once answers are captured and approved.
- Re-run blocked role (typically PLANNER/DEV/SECURITY/OPS/QA).

## 7) FAIL / #2 FAIL / ESCALATE rules

- First failed review for a milestone: verdict `FAIL`, baton back to `DEV_BUILDER` for fixes.
- Second fail for the same milestone (`failure_count >= 2`): must become `ESCALATE`.
- On `ESCALATE`, baton goes to `JUDGE_MEDIATOR` to split scope, re-lock decisions, or re-plan.
- After mediation, baton returns to `ARCHITECT`, `PLANNER`, or `DEV_BUILDER` per decision.

## 8) ITER-0001 success criteria checklist

- [ ] `ai/requirements.md` is concrete and implementation-ready.
- [ ] `ai/simplification.md` contains explicit 80/20 boundaries.
- [ ] `ai/decision-lock.yaml` is coherent and `confirmed_by_user: true`.
- [ ] `ai/plan.md` has 2-5 milestones with acceptance criteria.
- [ ] `ai/schema/schema.sql` reflects planned schema changes.
- [ ] Milestone 1 code/tests are implemented.
- [ ] Security/Ops/QA reviews are recorded in `ai/review.md`.
- [ ] Ratification is recorded in `ai/ratify.yaml` when QA passes.
- [ ] `ai/iterations/ITER-0001.md` contains role-by-role one-line decisions and clear baton flow.
