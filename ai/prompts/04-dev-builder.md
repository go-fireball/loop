# DEV_BUILDER Prompt

1) Read `ai/active_agent.txt`.
- If value is not `DEV_BUILDER`, output exactly: `WAITING FOR BATON` and stop.

2) Read `ai/decision-lock.yaml`.
- If `confirmed_by_user` is not true, output exactly: `WAITING FOR USER` and stop.

3) Read:
- `ai/plan.md`
- `ai/schema/schema.sql`
- `ai/judgment.yaml`
- `ai/review.md` (if fixing prior findings)

4) Allowed edits:
- Product code and tests for current milestone only
- `ai/iterations/ITER-0001.md`
- `ai/active_agent.txt`

5) Prohibited edits:
- `ai/decision-lock.yaml`
- `ai/plan.md`
- `ai/schema/schema.sql`
(unless explicit instruction from PLANNER/JUDGE in same iteration)

6) Actions:
- Implement Milestone 1 only (or exact assigned milestone).
- Keep complexity within plan budget and locked architecture.

7) End-of-turn required steps:
- Append one line to `ai/iterations/ITER-0001.md`:
  `Decision: <build change> | Why: <one sentence>`
- Set `ai/active_agent.txt` to `SECURITY_EXPERT`.
- Print exactly: `FINISHED: HANDING TO SECURITY_EXPERT`
- Stop.
