# loop

A lightweight governance + baton + escalation loop for software delivery across AI executors (`codex`, `claude`, `copilot`).

## What this repository is

Loop is intentionally small and file-driven:
- no heavy orchestration runtime
- no background daemon or multi-agent platform
- explicit baton ownership via files under `ai/`
- executor-agnostic runner contract

## Canonical contract (single source of truth)

### 1) Active roles

The active role set is:
- `PLANNER`
- `SENIOR_JUDGMENTAL_ENGINEER`
- `ENGINEER`
- `VALIDATOR`
- `HUMAN`

### 2) Default flow

Default happy path:

`PLANNER -> SENIOR_JUDGMENTAL_ENGINEER -> ENGINEER -> VALIDATOR`

`HUMAN` is escalation-only and is not part of the default happy path.

### 3) Baton files and canonical schema

- `ai/active_agent.txt` is the authoritative current role.
- `scripts/run-baton.sh` resolves role → prompt from a static mapping.
- `ai/next_agent.yaml` is baton metadata only (never prompt-routing config).

`ai/next_agent.yaml` schema (canonical):
- required:
  - `next_role`
- optional:
  - `handoff_notes`
  - `return_to` (allowed only when `next_role: HUMAN`; must be a non-`HUMAN` role)
  - `escalated_by`
  - `escalation_reason`

### 4) Terminal line contract

Agents must end with exactly one terminal line:
- `FINISHED: HANDING TO <ROLE>`
- `WAITING FOR USER`
- `WAITING FOR BATON`

### 5) HUMAN escalation and resume

Use `HUMAN` only for selective escalation, such as:
- unresolved ambiguity requiring business/context input
- role-level conflict not safely resolvable in-role
- validator failure needing explicit tradeoff/override

Escalation/resume behavior (canonical):
1. Escalating role writes `next_role: HUMAN` and includes `return_to` in `ai/next_agent.yaml`.
2. Runner pauses when terminal output is `WAITING FOR USER` and sets active role to `HUMAN`.
3. Human answers `ai/user-questions.yaml`.
4. Human runs `./scripts/resume-baton.sh`.
5. Next runner invocation resumes to `return_to`.

## Quick start (new project)

```bash
curl -sO https://raw.githubusercontent.com/go-fireball/loop/main/init.sh
chmod +x init.sh
./init.sh            # defaults to PLANNER
# optional explicit start role
./init.sh ENGINEER
```

## Start the loop (existing checkout)

1. Bootstrap state (defaults to `PLANNER` if omitted):
   ```bash
   ./scripts/bootstrap.sh
   # optional explicit start role
   ./scripts/bootstrap.sh ENGINEER
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

- `./scripts/bootstrap.sh [ROLE]` — seed `ai/` from `ai/defaults/`, initialize baton files, and set starting role (`PLANNER` default).
- `./scripts/check-baton.sh` — validate files, role validity, and the canonical `ai/next_agent.yaml` schema.
- `./scripts/generate-next-agent.sh <ROLE> [--notes ...] [--return-to ...] [--escalated-by ...] [--escalation-reason ...]` — write canonical baton metadata to `ai/next_agent.yaml`.
- `./scripts/resume-baton.sh [--force]` — mark HUMAN answers ready for runner resume to `return_to`.
- `./scripts/validate_baton.py` — YAML schema helper used by checks.
- Dependency note: YAML schema validation uses `PyYAML` (`python3 -m pip install pyyaml`).

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

## Migration note

For migration details from pre-escalation role chains, see `docs/baton-simplification.md`.
