# Simplification Output (Senior Judgmental Engineer)

## Simplification Plan
- What to cut, reduce, or avoid in the current phase.
- Prioritize the smallest implementation that delivers core value.

## Deferred Complexity
- List features, integrations, or architecture choices intentionally postponed.
- Include short reason for each deferred item.

## Complexity Budget
- Explicit constraints for current phase (example defaults):
  - single deployable
  - containers-only
  - no new infra class without explicit confirmation

## Exit Criteria to Add Complexity Later
- Conditions that must be true before adding deferred complexity.
- Use measurable triggers where possible (load, reliability, compliance, business need).

## Open Questions (Only If Truly Blocking)
- Include only unresolved questions that block progress now.
