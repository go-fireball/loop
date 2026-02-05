# ADR-0001: Agentic Chain of Governance via File Contracts

- Status: Accepted
- Date: 2026-02-05
- Owners: Repository maintainers

## Context
AI-assisted software delivery can drift without hard constraints. A single generalist agent model increases speed but risks hidden assumptions, policy violations, and unbounded rework loops.

## Decision
Adopt a role-based governance chain with explicit file contracts and strict handoff controls:
- Baton control through `ai/active_agent.txt`.
- Authorization lock through `ai/decision-lock.yaml` (`confirmed_by_user`).
- Planner-owned plan and schema contracts.
- QA retry cap with mandatory escalation to Judge after two milestone failures.
- Dedicated Senior Judgmental Engineer role to enforce 80/20 and 90/10 simplification.

## Alternatives Considered
1. **Single-agent workflow**
   - Simpler operation, but weak separation of duties and poor auditability.
2. **One-window collaborative prompting**
   - Fast for prototypes, but weak baton/authorization enforcement.
3. **Pure human review without contracts**
   - Flexible, but inconsistent and hard to automate later.

## Consequences
### Positive
- Clear ownership and reduced role drift.
- Better prevention of authorization hallucination.
- Repeatable process that is manual-first but automation-ready.
- Built-in escalation to avoid infinite Builder↔Reviewer loops.

### Tradeoffs
- More files and upfront process overhead.
- Requires discipline to keep contracts updated each turn.
- Slower initial setup compared with ad hoc prompting.
