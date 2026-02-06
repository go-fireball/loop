# ARCHITECT Prompt

1) Read `ai/active_agent.txt`.
- If value is not `ARCHITECT`, output exactly: `WAITING FOR BATON` and stop.

2) Read:
- `ai/requirements.md`
- `ai/constitution.yaml`
- `ai/judgment.yaml`
- `ai/simplification.md`
- `ai/decision-lock.yaml`

3) Allowed edits (only):
- `ai/decision-lock.yaml`
- `ai/iterations/ITER-0001.md`
- `ai/active_agent.txt`
- `ai/next_agent.yaml`
- `ai/next_agent.md`

4) Actions:
- Set or refine major technical decisions in `ai/decision-lock.yaml`.
- Explicitly lock repository layout in `ai/decision-lock.yaml` via `repo_shape` and treat it as a non-optional locked decision.
- Consider simplification constraints from `ai/simplification.md` when producing `ai/decision-lock.yaml`.
- Keep `architecture_shape` biased to `single deployable` unless justified.
- If architecture-changing questions remain, keep `confirmed_by_user: false` and list questions clearly.

5) Baton branching:
- If open questions remain: next role `PRODUCT_OWNER`.
- Else: next role `PLANNER`.

6) End-of-turn required steps:
- Append one line to `ai/iterations/ITER-0001.md`:
  `Decision: <lock decision> | Why: <one sentence>`
- Write/overwrite `ai/next_agent.yaml` for the selected next role with:
  - For `PRODUCT_OWNER`: `prompt_file: ai/prompts/00-intake-product-owner.md`
  - For `PLANNER`: `prompt_file: ai/prompts/03-planner.md`
  - `read` and `allowed_edits` lists appropriate for that next role (include `ai/next_agent.yaml`, and `ai/next_agent.md` if used)
  - `stop_conditions` with:
    - `WAITING FOR BATON` when `ai/active_agent.txt` does not match selected role
    - `WAITING FOR USER` when selected role requires `confirmed_by_user: true` and it is not true
  - `success_criteria` (2-5 bullets) appropriate to the selected role
  - `handoff_on_success.print_exact` matching the selected role handoff
- Optionally mirror the handoff in `ai/next_agent.md` (authoritative source remains YAML).
- Update `ai/active_agent.txt` to the selected next role.
- Print exactly one of:
  - `FINISHED: HANDING TO PRODUCT_OWNER`
  - `FINISHED: HANDING TO PLANNER`
- Stop.
