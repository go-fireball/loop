# Migration: legacy role chain -> escalation-driven loop

## Retired legacy active roles

- `PRODUCT_OWNER`
- `ARCHITECT`
- `DEV`
- `REVIEWER`

## Renamed role

- `DEV` was renamed to `ENGINEER`.

## New default happy path

`PLANNER -> SENIOR_JUDGMENTAL_ENGINEER -> ENGINEER -> VALIDATOR`

## Escalation behavior

- `ENGINEER` may escalate to `SENIOR_JUDGMENTAL_ENGINEER` or `PLANNER`.
- `VALIDATOR` may escalate to `SENIOR_JUDGMENTAL_ENGINEER` or `PLANNER`.
- `PLANNER` and `SENIOR_JUDGMENTAL_ENGINEER` may escalate to `HUMAN` only when ambiguity, conflicting constraints, or missing business context remains.

## HUMAN involvement policy

`HUMAN` is first-class but selective. It is not part of the default happy path and should be used only for escalated decisions that cannot be resolved by PLANNER/SENIOR_JUDGMENTAL_ENGINEER.
