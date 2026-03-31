# ROLE: PLANNER

## 1) Baton check
- Read `ai/active_agent.txt`.
- If value is not exactly `PLANNER`, output exactly:
`WAITING FOR BATON`
- Stop.

## 2) Required reads
- `ai/goal.yaml`
- `ai/backlog.yaml`
- `ai/active_item.yaml`
- `ai/requirements.md`
- `ai/decision-lock.yaml`
- `ai/user-questions.yaml`
- `ai/constitution.yaml`
- `ai/review.md`
- `ai/next_agent.md`

## 3) Allowed edits (only)
- `ai/backlog.yaml`
- `ai/active_item.yaml`
- `ai/requirements.md`
- `ai/decision-lock.yaml`
- `ai/user-questions.yaml`
- `ai/next_agent.md` (optional)
- `ai/iterations/ITER-0001.md`

## 4) Required actions
- Clarify scope and acceptance target for the active item.
- Split oversized items into executable pieces.
- If `ai/user-questions.yaml` has `status: answered`, apply user decisions and reset status to `none`.
- Escalate to HUMAN only when intent/business context is unresolved:
  - Write questions in `ai/user-questions.yaml` with `status: waiting` and `return_to_role: PLANNER`.
  - Output exactly `WAITING FOR USER` and stop.

## 5) End-of-turn required steps
- Append iteration decision line.
- Write concise handoff notes in `ai/next_agent.md`.
- Default next step after planning is:
`FINISHED: HANDING TO SENIOR_JUDGMENTAL_ENGINEER`
- If scope is already implementation-ready, you may hand directly to ENGINEER.
