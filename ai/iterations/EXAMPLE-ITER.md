# Iteration Log: ITER-EXAMPLE

## Iteration Header
- plan_id: ITER-EXAMPLE
- purpose: Teaching artifact showing concise, high-signal role entries and review flow.

## Worked Example Entries
- [SEQ-001] [SENIOR_JUDGMENTAL_ENGINEER] Decision: Ship only task CRUD + list filter in Milestone 1; defer analytics/export. | Why: 80/20 cut removes non-core complexity while validating core workflow first.
- [SEQ-002] [ARCHITECT] Decision: Lock architecture to single deployable web app + relational DB (no event bus). | Why: Reduces operational overhead and keeps design aligned with early-stage requirements.
- [SEQ-003] [PLANNER] Decision: Define Milestone 1 as API + UI CRUD + auth guard; include initial `tasks` schema in `ai/schema/schema.sql`. | Why: Delivers end-to-end vertical slice with testable acceptance criteria.
- [SEQ-004] [DEV_BUILDER] Decision: Implemented Milestone 1 endpoints, DB model, and basic task list/create UI with tests. | Why: Meets milestone scope without introducing deferred complexity.
- [SEQ-005] [QA_REVIEWER_RATIFIER] Decision: FAIL (#1) — auth invariant violation: update endpoint allowed unauthenticated access. | Why: Violates locked security invariant requiring authenticated task mutation.
- [SEQ-006] [DEV_BUILDER] Decision: Added auth middleware to update/delete endpoints and regression tests for 401 behavior. | Why: Closes cited invariant gap and prevents recurrence.
- [SEQ-007] [QA_REVIEWER_RATIFIER] Decision: PASS + ratified=true for Milestone 1 after re-test of auth, CRUD flow, and schema checks. | Why: Acceptance criteria and invariants now satisfied.

## Simulated Escalation Snippet (for teaching only)

> Simulated (not from live run):
>
> - [SIM-FAIL-1] QA verdict: FAIL for missing rollback note in release instructions (failure_count=1).
> - [SIM-FAIL-2] QA verdict: FAIL again for same milestone after partial fix (failure_count=2).
> - [SIM-ESCALATE] QA verdict: ESCALATE; baton handed to JUDGE_MEDIATOR per retry rule.
> - [SIM-JUDGE] JUDGE_MEDIATOR decision: split milestone, move release automation to next milestone, return baton to PLANNER.
