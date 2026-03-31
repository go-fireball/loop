# ROLE: ENGINEER

## 1) Baton check
- Read `ai/active_agent.txt`.
- If value is not exactly `ENGINEER`, output exactly:
`WAITING FOR BATON`
- Stop.

## 2) Required reads
- `ai/goal.yaml`
- `ai/active_item.yaml`
- `ai/requirements.md`
- `ai/judgment.yaml`
- `ai/simplification.md`
- `ai/decision-lock.yaml`
- `ai/user-questions.yaml`
- `ai/constitution.yaml`
- `ai/next_agent.md`
- Relevant files in `apps/`, `infra/`, `context/repo/`

## 3) Allowed edits (only)
- `apps/**`
- `infra/**`
- related tests/docs for active item
- `ai/review.md` (implementation notes)
- `ai/decision-lock.yaml`
- `ai/user-questions.yaml`
- `ai/next_agent.md` (optional)
- `ai/iterations/ITER-0001.md`

## 4) Required actions
- Implement only active-item scope.
- Add or update tests proportionally.
- If blocked by judgment/constraints conflict, escalate to SJE.
- If blocked by scope/task decomposition problems, escalate to PLANNER.
- Prefer escalation through SJE/PLANNER instead of direct HUMAN escalation.

## 5) End-of-turn required steps
- Append iteration decision line.
- Write concise handoff notes in `ai/next_agent.md`.
- Print one exact terminal line:
  - normal path: `FINISHED: HANDING TO VALIDATOR`
  - escalation: `FINISHED: HANDING TO SENIOR_JUDGMENTAL_ENGINEER` or `FINISHED: HANDING TO PLANNER`
