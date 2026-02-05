# AI Governance Folder (`/ai`)

This folder is the operating system for agentic delivery. It keeps each role constrained, auditable, and aligned.

## File Contracts (Single Source of Truth)
- `requirements.md`: one-page requirement authored by Product Owner.
- `constitution.yaml`: non-negotiable policy constraints (budget, privacy, safety).
- `judgment.yaml`: engineering invariants, mode, infra ceiling, and done criteria.
- `decision-lock.yaml`: frozen major decisions and `confirmed_by_user` authorization lock.
- `plan.md`: milestone-level execution contract and deferred complexity tracking.
- `schema/schema.sql`: planned schema changes (Planner-owned only).
- `review.md`: review verdict and punch list with cited violations.
- `ratify.yaml`: legal/ratification state for milestone acceptance.
- `active_agent.txt`: baton control; only listed role may act.
- `iterations/ITER-0001.md`: decision log line appended by every role each turn.

## Baton Rule (Hard Stop)
Every role prompt starts by reading `ai/active_agent.txt`.
- If not your role: output exactly `WAITING FOR BATON` and stop.
- If your role: do your owned edits only, append one-line decision log, set next role in `active_agent.txt`, print `FINISHED: HANDING TO <NEXT_ROLE>`, then stop.

## Authorization Lock Rule (Hard Stop)
If `ai/decision-lock.yaml` has `confirmed_by_user: false`, these roles must stop and output exactly `WAITING FOR USER`:
- PLANNER
- DEV_BUILDER
- SECURITY_EXPERT
- OPS_SRE
- QA_REVIEWER_RATIFIER

Only authorized roles may set `confirmed_by_user: true` under policy (typically Product Owner, Senior Judgmental Engineer if allowed, Architect/Judge where constitution allows).

## Retry Limit and Escalation
- QA tracks `failure_count_for_milestone` in `ai/review.md`.
- On second failure of same milestone: verdict must be `ESCALATE`.
- Baton must pass to `JUDGE_MEDIATOR` instead of cycling endlessly with Dev.

## Accepted Locally vs Ratified as Law
- **Accepted locally**: implementation appears to work for one role.
- **Ratified as law**: `ai/ratify.yaml` says `ratified: true` after QA confirms lock/invariants/constitution/tests/docs/no-infra-creep.

## Running Agents with Prompts
Use prompts in order from `ai/prompts/` for manual multi-window operation:
1. Product Owner intake
2. Senior Judgmental Engineer simplification
3. Architect decision lock
4. Planner plan + schema
5. Dev build (milestone by milestone)
6. Security review
7. Ops/SRE review
8. QA review + ratify
9. Judge/Mediator on escalation
