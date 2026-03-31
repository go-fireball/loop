# Baton Simplification

## Goal

Make baton handling deterministic by separating **state** from **behavior**.

- State answers: who runs next?
- Behavior answers: what does that role do?

## Source of truth

- `ai/active_agent.txt`: authoritative current role.
- `scripts/run-baton.sh`: authoritative role→prompt resolution.

`ai/next_agent.yaml` is not used to resolve role prompts or execution behavior.

## File roles

- `ai/active_agent.txt`
  - Single source of truth for current role.
- `ai/next_agent.yaml`
  - Optional, minimal baton metadata:
    - `next_role` (required when file exists)
    - `handoff_notes` (optional)
    - `return_to` (optional)
    - `escalated_by` (optional)
    - `escalation_reason` (optional)
- `ai/next_agent.md`
  - Optional narrative handoff context only.
- `scripts/generate-next-agent.sh`
  - Generates `ai/next_agent.yaml` with only canonical baton keys (`next_role`, optional `handoff_notes`, `return_to`, `escalated_by`, `escalation_reason`).
- `scripts/check-baton.sh`
  - Validates active role and minimal baton schema; does not validate prompt behavior from YAML.
- `scripts/run-baton.sh`
  - Reads active role from `ai/active_agent.txt`, resolves prompt via static mapping, executes, parses strict terminal contract, updates baton.

## Active roles and default flow

Active roles: `PLANNER`, `SENIOR_JUDGMENTAL_ENGINEER`, `ENGINEER`, `VALIDATOR`, `HUMAN`.

Default happy path:

`PLANNER -> SENIOR_JUDGMENTAL_ENGINEER -> ENGINEER -> VALIDATOR`

`HUMAN` is used via escalation, not as a mandatory checkpoint.

## Strict handoff contract

Agents must end with exactly one of:

- `FINISHED: HANDING TO <ROLE>`
- `WAITING FOR USER`
- `WAITING FOR BATON`

## Baton flow

1. Runner reads current role from `ai/active_agent.txt`.
2. Runner resolves prompt from static role map.
3. Runner executes AI turn.
4. Runner parses terminal contract line:
   - `FINISHED: HANDING TO <ROLE>`
     - update `ai/active_agent.txt` to `<ROLE>`
     - generate minimal `ai/next_agent.yaml` for `<ROLE>`
   - `WAITING FOR USER`
     - set `ai/active_agent.txt` to `HUMAN`
     - generate minimal `ai/next_agent.yaml` with `next_role: HUMAN`, `return_to`, and escalation metadata
   - `WAITING FOR BATON`
     - stop with no baton transition
