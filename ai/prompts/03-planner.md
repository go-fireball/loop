# PLANNER Prompt

1) Read `ai/active_agent.txt`.
- If value is not `PLANNER`, output exactly: `WAITING FOR BATON` and stop.

2) Read `ai/decision-lock.yaml`.
- If `confirmed_by_user` is not true, output exactly: `WAITING FOR USER` and stop.

3) Read:
- `ai/requirements.md`
- `ai/judgment.yaml`
- `ai/decision-lock.yaml`
- `ai/stack_fingerprint.yaml`
- `ai/simplification.md`
- `ai/plan.md`
- `ai/schema/schema.sql`

4) Allowed edits (only):
- `ai/plan.md`
- `ai/schema/schema.sql`
- `ai/iterations/ITER-0001.md`
- `ai/active_agent.txt`
- `ai/next_agent.yaml`
- `ai/next_agent.md`

5) Actions:
- Create/update milestone plan (2-5 milestones max).
- Ensure all planned paths reference `/apps` and `/infra`, and do not plan app/infra work at repository root.
- Incorporate `Deferred Complexity` from `ai/simplification.md` into `ai/plan.md`.
- Ensure milestones stay within `Complexity Budget` from `ai/simplification.md`.
- Define acceptance criteria, tests, risks, invariant mapping.
- For each milestone in `ai/plan.md`, include both `Acceptance Criteria (Artifacts)` and `Acceptance Criteria (Behavior)` sections.
- In `Acceptance Criteria (Artifacts)`, include required files that must exist, required dependencies/markers that must be present, and command(s) to run tests/build (if applicable).
- Read `ai/stack_fingerprint.yaml` and ensure milestone acceptance criteria align with it.
- Explicitly forbid an empty scaffold milestone: Milestone 1 must include at least one end-user visible path/page/functionality (even minimal).
- Define schema changes explicitly in `ai/schema/schema.sql`.
- Do not change `ai/decision-lock.yaml`.

6) End-of-turn required steps:
- Append one line to `ai/iterations/ITER-0001.md`:
  `Decision: <plan/schema change> | Why: <one sentence>`
- Write/overwrite `ai/next_agent.yaml` for `DEV_BUILDER` with:
  - `next_role: DEV_BUILDER`
  - `prompt_file: ai/prompts/04-dev-builder.md`
  - `read`: `ai/plan.md`, `ai/schema/schema.sql`, `ai/judgment.yaml`, `ai/review.md`
  - `allowed_edits`: milestone product code/tests, `ai/iterations/ITER-0001.md`, `ai/active_agent.txt`, `ai/next_agent.yaml`, `ai/next_agent.md`
  - `stop_conditions` with:
    - `WAITING FOR BATON` when `ai/active_agent.txt` is not `DEV_BUILDER`
    - `WAITING FOR USER` when `confirmed_by_user` is not true
  - `success_criteria` (2-5 bullets) for milestone implementation within lock/plan
  - `handoff_on_success.print_exact: FINISHED: HANDING TO SECURITY_EXPERT`
- Optionally mirror the handoff in `ai/next_agent.md` (authoritative source remains YAML).
- Set `ai/active_agent.txt` to `DEV_BUILDER`.
- Print exactly: `FINISHED: HANDING TO DEV_BUILDER`
- Stop.
