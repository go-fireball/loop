# Schema Ownership Rules

- `ai/schema/schema.sql` is Planner-owned.
- Dev/Builder must not introduce schema changes not explicitly defined in `ai/plan.md`.
- Any schema diff must be recorded here before implementation.
- Security/Ops/QA validate schema changes against constitution, judgment invariants, and decision lock.
- If schema needs change but is not planned, baton returns to PLANNER.
