# AI Judgment Workflow (Files-as-Contracts)

This folder defines a guardrailed, multi-agent workflow using files as the source of truth.

## Files
- `ai/judgment.yaml`: machine-readable judgment and invariants that set scope and mode.
- `ai/decision-lock.yaml`: frozen stack and infra choices; only Judge/Ratifier may edit.
- `ai/plan.md`: milestone plan derived from the confirmed decision lock.
- `ai/review.md`: reviewer output with PASS/FAIL and punch list.
- `ai/ratify.yaml`: final law of merge; must be `ratified: true` to merge.

## Intended flow (two keys to merge)
1. Judge sets judgment and locks decisions.
2. Planner writes the plan based on the lock.
3. Builder implements Milestone 1 only.
4. Reviewer checks against judgment + lock + plan.
5. Ratifier updates `ai/ratify.yaml` (review + ratify = two keys).

## How to run each agent
Use prompts in `ai/prompts/` to run each role in order:
- A-judge.md → B-planner.md → C-builder.md → D-reviewer.md → E-ratifier.md

## Rules
- Only Judge/Ratifier can modify `ai/decision-lock.yaml` and `ai/ratify.yaml`.
- No plan without confirmed decision lock.
- No build without plan.
