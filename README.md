# loop

A lightweight, file-based baton loop for AI-assisted software delivery. Loop is executor-agnostic (`codex`, `claude`, `copilot`) and intentionally governance-centric: it coordinates judgment, escalation, and handoffs without adding heavy orchestration.

## Core philosophy

- Keep state in files under `ai/`.
- Keep baton ownership explicit and interruptible.
- Keep execution depth in the chosen model, not in framework machinery.
- Use HUMAN selectively for unresolved business/context decisions.

## Canonical role model

Active roles:

- `PLANNER`
- `SENIOR_JUDGMENTAL_ENGINEER`
- `ENGINEER`
- `VALIDATOR`
- `HUMAN`

Default happy path:

`PLANNER -> SENIOR_JUDGMENTAL_ENGINEER -> ENGINEER -> VALIDATOR`

`HUMAN` is **not** in the default path; HUMAN is entered only via escalation.

## Baton contract

Authoritative state:

- `ai/active_agent.txt` — current role (source of truth)
- `ai/next_agent.yaml` — baton metadata for next handoff

`ai/next_agent.yaml` schema:

- `next_role` (required)
- `handoff_notes` (optional)
- `return_to` (optional)
- `escalated_by` (optional)
- `escalation_reason` (optional)

Strict terminal contract per turn:

- `FINISHED: HANDING TO <ROLE>`
- `WAITING FOR USER`
- `WAITING FOR BATON`

## Escalation model

Preferred escalation paths:

- `ENGINEER -> SENIOR_JUDGMENTAL_ENGINEER`
- `ENGINEER -> PLANNER`
- `VALIDATOR -> SENIOR_JUDGMENTAL_ENGINEER`
- `VALIDATOR -> PLANNER`
- `SENIOR_JUDGMENTAL_ENGINEER -> HUMAN`
- `PLANNER -> HUMAN`

Direct `ENGINEER -> HUMAN` and `VALIDATOR -> HUMAN` are blocked by runner validation; escalate through `SENIOR_JUDGMENTAL_ENGINEER` or `PLANNER`.

## Setup

### New project

```bash
curl -sO https://raw.githubusercontent.com/go-fireball/loop/main/init.sh
chmod +x init.sh
./init.sh                 # defaults to PLANNER
# or
./init.sh ENGINEER
```

### Existing checkout

```bash
./scripts/bootstrap.sh PLANNER
# edit ai/goal.yaml with your real objective and constraints
./scripts/check-baton.sh
./scripts/run-baton.sh --executor <codex|claude|copilot>
```

## Human pause/resume

When a role outputs `WAITING FOR USER`:

1. Runner sets `ai/active_agent.txt` to `HUMAN`.
2. Runner writes `ai/next_agent.yaml` with `next_role: HUMAN`, `return_to`, and escalation metadata.
3. Human answers `ai/user-questions.yaml`.
4. Human runs `./scripts/resume-baton.sh`.
5. Next `run-baton.sh` resumes to `return_to`.

## Script reference

- `./scripts/bootstrap.sh [ROLE]` — seeds `ai/` from `ai/defaults/`, creates baton files.
- `./scripts/check-baton.sh` — validates required files, role values, and baton schema.
- `./scripts/generate-next-agent.sh <ROLE> [--notes ...] [--return-to ...] [--escalated-by ...] [--escalation-reason ...]` — writes `ai/next_agent.yaml` with only the canonical schema fields.
- `./scripts/resume-baton.sh [--force]` — marks human answers ready.
- `./scripts/run-baton.sh --executor <codex|claude|copilot>` — main baton runner.
- `./scripts/validate_baton.py` — YAML structure validator.

## Legacy prompt handling

Legacy active-role prompts are not part of the active flow and are not retained as active prompt mappings. The active prompt set is:

- `ai/defaults/prompts/00-planner.md`
- `ai/defaults/prompts/01-senior-judgmental-engineer.md`
- `ai/defaults/prompts/02-engineer.md`
- `ai/defaults/prompts/03-validator.md`
- `ai/defaults/prompts/human.md`

## Migration note

For legacy-role migration details (retired roles, rename, escalation policy), see `docs/migration-legacy-roles.md`.
