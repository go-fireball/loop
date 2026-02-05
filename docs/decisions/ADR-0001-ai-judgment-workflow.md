# ADR-0001: AI Judgment Workflow

## Context
Work needs clear guardrails so multiple agents can collaborate without drifting scope or stack. Decisions must be auditable and stable across planning, build, and review.

## Decision
Adopt a multi-agent workflow where key contracts live in files:
- `ai/judgment.yaml` defines scope, invariants, and mode.
- `ai/decision-lock.yaml` freezes stack and infra choices.
- `ai/plan.md`, `ai/review.md`, and `ai/ratify.yaml` enforce sequential gates.

## Alternatives
- Single-agent workflow with chat-based memory.
- Informal decisions tracked in issues only.

## Consequences
- Clear separation of roles reduces drift and rework.
- Files become the source of truth and must be kept consistent.
- Merges require review plus ratification.
