# Contributing

This repository supports both human contributors and AI agents. Follow these governance rules strictly.

## Mandatory Rules
1. **No plan -> no code.**
   - Implementation starts only after `ai/plan.md` exists for the active iteration.
2. **No confirmed lock -> no plan/build/review.**
   - If `ai/decision-lock.yaml` is not confirmed, Planner/Dev/Security/Ops/QA must stop with `WAITING FOR USER`.
3. **Milestone-by-milestone delivery only.**
   - Build one milestone at a time; avoid scope blending.
4. **Schema is Planner-owned.**
   - Schema changes must be declared in `ai/schema/schema.sql` before implementation.
5. **Review and ratify are required.**
   - `ai/review.md` and `ai/ratify.yaml` must be updated before milestone closure.
6. **Escalation after two fails.**
   - QA must escalate to Judge/Mediator when a milestone fails twice.

## Operating Workflow
- Use prompts in `ai/prompts/` in baton order.
- Respect `ai/active_agent.txt` as the only authority for who acts.
- Append one-line `Decision | Why` entries in `ai/iterations/ITER-0001.md` every role turn.

## Pull Requests
Use `.github/PULL_REQUEST_TEMPLATE.md` to verify governance artifacts are complete and consistent.
