# Quality Bar

## Single Judgment Loop
Use one shared judgment contract (`ai/judgment.yaml`) for all roles. This prevents role-specific standards from diverging and keeps quality decisions coherent.

## Mode-Specific Strictness

### PoC Mode
- Goal: validate feasibility quickly.
- Strictness: minimal but explicit safety and traceability.
- Expected: basic tests and lightweight docs.

### MVP Mode
- Goal: deliver usable value safely.
- Strictness: enforce invariants, review gates, and ratification.
- Expected: milestone-level acceptance tests, security/ops checks, clear rollback notes.

### Enterprise Mode
- Goal: production-grade governance at scale.
- Strictness: expanded compliance, reliability, and audit controls.
- Expected: stronger SLO/SLA evidence, deeper security modeling, formal change controls.

## Always-On Quality Rules
- Decision lock must be confirmed before planning/build/review roles proceed.
- No infra creep beyond declared `infra_level` without explicit lock update.
- Schema changes must originate in Planner-owned schema contract.
- QA ratification is required before milestone is considered law.

## Reference
- Source of truth: `ai/judgment.yaml`.
