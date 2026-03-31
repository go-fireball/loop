# loop

A lightweight governance + baton + escalation loop for software delivery across AI executors (`codex`, `claude`, `copilot`).

## What this repository is

Loop is intentionally small and file-driven:
- no heavy orchestration runtime
- no background daemon or multi-agent platform
- explicit baton ownership via files under `ai/`
- executor-agnostic runner contract

## Core contract

1. `ai/active_agent.txt` is the authoritative current role.
2. `scripts/run-baton.sh` resolves role → prompt from a static mapping.
3. `ai/next_agent.yaml` is minimal baton metadata with one required key and four optional keys:
   - required: `next_role`
   - optional: `handoff_notes`, `return_to` (only when `next_role: HUMAN`), `escalated_by`, `escalation_reason`
4. Agents must end with exactly one terminal line:
   - `FINISHED: HANDING TO <ROLE>`
   - `WAITING FOR USER`
   - `WAITING FOR BATON`

## Role model (active flow)

First-class roles:
- `PLANNER`
- `SENIOR_JUDGMENTAL_ENGINEER`
- `ENGINEER`
- `VALIDATOR`
- `HUMAN`

Default happy path:

`PLANNER -> SENIOR_JUDGMENTAL_ENGINEER -> ENGINEER -> VALIDATOR`

## Escalation-first baton behavior

Loop is event-driven, not checkpoint-heavy.

Allowed escalation patterns:
- `ENGINEER -> SENIOR_JUDGMENTAL_ENGINEER`
- `VALIDATOR -> SENIOR_JUDGMENTAL_ENGINEER`
- `ENGINEER -> PLANNER` (scope/decomposition mismatch)
- `VALIDATOR -> PLANNER` (acceptance/scope ambiguity)
- `SENIOR_JUDGMENTAL_ENGINEER -> HUMAN`
- `PLANNER -> HUMAN`

`ENGINEER`/`VALIDATOR` should usually escalate through `SENIOR_JUDGMENTAL_ENGINEER` or `PLANNER` rather than directly to `HUMAN`.

## HUMAN involvement policy

`HUMAN` is selective escalation authority, not a mandatory step after each role.

Use `HUMAN` when:
- unresolved ambiguity needs business/context input
- role-level conflict cannot be resolved safely
- validator failure requires tradeoff/override

Resume behavior:
- Runner pauses on `WAITING FOR USER` and sets active role to `HUMAN`.
- Human answers `ai/user-questions.yaml`.
- Human runs `./scripts/resume-baton.sh`.
- Next runner invocation resumes to `return_to` in `ai/next_agent.yaml`.

## Quick start (new project)

```bash
curl -sO https://raw.githubusercontent.com/go-fireball/loop/main/init.sh
chmod +x init.sh
./init.sh            # defaults to PLANNER
# or
./init.sh ENGINEER   # start from a different role
```

## Start the loop (existing checkout)

1. Bootstrap state:
   ```bash
   ./scripts/bootstrap.sh PLANNER
   ```
2. Update `ai/goal.yaml` for your project.
3. Validate baton state:
   ```bash
   ./scripts/check-baton.sh
   ```
4. Run:
   ```bash
   ./scripts/run-baton.sh --executor <codex|claude|copilot>
   ```

## CLI reference

- `./scripts/bootstrap.sh [ROLE]` — seed `ai/` from `ai/defaults/` and initialize baton files.
- `./scripts/check-baton.sh` — validate files, role validity, and `ai/next_agent.yaml` schema (including optional escalation keys).
- `./scripts/generate-next-agent.sh <ROLE> [--notes ...] [--return-to ...] [--escalated-by ...] [--escalation-reason ...]` — write baton metadata with the same schema documented above.
- `./scripts/resume-baton.sh [--force]` — mark HUMAN answers ready for runner resume.
- `./scripts/validate_baton.py` — YAML schema helper used by checks.

## Runner commands

```bash
./scripts/run-baton.sh --executor claude
./scripts/run-baton.sh --executor codex --max-steps 5
./scripts/run-baton.sh --executor copilot --dry-run
```

Supported executors:
- `codex` (default model: `gpt-5.4`)
- `claude` (default model: `claude-sonnet-4-6`)
- `copilot` (default model: `claude-sonnet-4-6`)

## Why this model is lighter

Loop focuses on governance and baton clarity, while leaving deep execution/orchestration capabilities to external executors. This keeps the system:
- simple
- interruptible
- human-compatible
- executor-agnostic

## Migration note (old -> new)

Historical mapping for teams migrating from the prior flow:
- Removed from active flow: `PRODUCT_OWNER`, `ARCHITECT`, `DEV`, `REVIEWER`.
- Renamed active execution role: `DEV` -> `ENGINEER`.
- New default path: `PLANNER -> SENIOR_JUDGMENTAL_ENGINEER -> ENGINEER -> VALIDATOR`.
- Escalation now drives routing decisions instead of rigid role checkpoints.
- `HUMAN` is selective and escalation-based, with resume to `return_to`.
