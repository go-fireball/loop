## Agentic Governance Checklist

### Contract Inputs
- [ ] `ai/requirements.md` is present and current.
- [ ] `ai/constitution.yaml` constraints were acknowledged.
- [ ] `ai/judgment.yaml` invariants/non-goals were respected.

### Lock and Plan
- [ ] `ai/decision-lock.yaml` has `confirmed_by_user: true` for plan/build/review phases.
- [ ] `plan_id` and milestone are identified (`ai/plan.md`).
- [ ] `ai/schema/schema.sql` updated if schema changes were required.

### Review and Ratification
- [ ] `ai/review.md` includes verdict and citations.
- [ ] `ai/ratify.yaml` status is set and consistent with verdict.
- [ ] Retry/escalation rule was applied when failures repeated.

### Quality and Drift Control
- [ ] Invariant coverage is documented.
- [ ] No infra creep beyond lock/judgment policy.
- [ ] Decision log line appended in `ai/iterations/ITER-0001.md`.
