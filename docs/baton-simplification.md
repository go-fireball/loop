# Baton Simplification

## Goal

Keep baton handling deterministic by separating state from behavior.

- State answers: who runs next?
- Behavior answers: what does that role do?

## Source of truth

- `ai/active_agent.txt`: authoritative current role.
- `scripts/run-baton.sh`: authoritative role→prompt resolution.

`ai/next_agent.yaml` is baton metadata only and never prompt-routing config.

## Minimal baton schema

`ai/next_agent.yaml` supports only:
- `next_role` (required)
- `handoff_notes` (optional)
- `return_to` (optional; only when `next_role: HUMAN`)
- `escalated_by` (optional)
- `escalation_reason` (optional)

## Active role set

- `PLANNER`
- `SENIOR_JUDGMENTAL_ENGINEER`
- `ENGINEER`
- `VALIDATOR`
- `HUMAN`

Default happy path:

`PLANNER -> SENIOR_JUDGMENTAL_ENGINEER -> ENGINEER -> VALIDATOR`

## Escalation-first flow

Routing is event-driven. Any role may hand off by `FINISHED: HANDING TO <ROLE>`.

Common escalation paths:
- `ENGINEER -> SENIOR_JUDGMENTAL_ENGINEER`
- `VALIDATOR -> SENIOR_JUDGMENTAL_ENGINEER`
- `ENGINEER -> PLANNER`
- `VALIDATOR -> PLANNER`
- `SENIOR_JUDGMENTAL_ENGINEER -> HUMAN`
- `PLANNER -> HUMAN`

When waiting for human input, role prints `WAITING FOR USER`; runner sets baton owner to HUMAN and stores `return_to` for resume.

## Strict terminal contract

Agents must end with exactly one of:
- `FINISHED: HANDING TO <ROLE>`
- `WAITING FOR USER`
- `WAITING FOR BATON`

## Why mismatch is prevented

- Execution role always comes from `ai/active_agent.txt`.
- Prompt file always comes from static runner map.
- Generated YAML cannot reroute behavior.
