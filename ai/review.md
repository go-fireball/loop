# Review Report

- verdict: PASS | FAIL | ESCALATE
- milestone: <MILESTONE_ID>

## Summary
- Brief statement of current review outcome.

## Violations (with citations)
- Cite exact source for each issue:
  - constitution: ai/constitution.yaml
  - invariants/non-goals: ai/judgment.yaml
  - locked decisions: ai/decision-lock.yaml
  - plan and schema contracts: ai/plan.md, ai/schema/schema.sql

## Security Findings
- [Pass/Fail notes]

## Ops/SRE Findings
- [Pass/Fail notes]

## Required Fixes (Punch List)
1. 
2. 

## Test / Documentation Gaps
- 

## Retry Tracking
- failure_count_for_milestone: 0
- retry_rule: "If this reaches 2 for the same milestone, set verdict ESCALATE and hand baton to JUDGE_MEDIATOR."

## Next Baton
- next_baton: DEV_BUILDER | JUDGE_MEDIATOR | PLANNER
