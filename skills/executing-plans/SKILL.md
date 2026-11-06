---
name: executing-plans
description: Use to implement an independently approved plan natively, task by task, in the current conversation; send code review gates to GPT-6.1 Sol High through T3 Code.
---

# Executing Plans — Native

**One implementer:** You. Make all code edits, test runs, commits and fixes directly in this conversation and the existing project checkout. Do not hand implementation tasks to other agents. Do not offer or ask about alternate execution methods.

## Preparation

1. Read the approved design, plan and the T3 review-log verdict. If the plan is not independently approved, invoke `superpowers:t3-model-gates` to obtain that decision first.
2. Check `git status --short`, current branch, and the project's baseline test command. Preserve unrelated edits. Do not create another checkout or switch project roots to implement.
3. Create `docs/superpowers/implementation-log.md` (or another existing project log) to track task numbers, revision SHAs, test results, deviations and reviewer decisions. Keep the log current.
4. Load `superpowers:test-driven-development` and `superpowers:verification-before-completion`. Flag any plan contradictions, check against the design, and resolve technical uncertainty before changing code.

## Native task loop

For every numbered task:

1. Re-read the task requirements, interfaces, file paths and expected test failures. Note the base git revision and related state.
2. Write the failing test first, run it, and confirm it fails for the expected reason. A test that passes before the change does not prove the new behavior.
3. Implement the smallest correct change, refactor if necessary, and run the named tests. Apply systematic debugging for unexpected failures rather than patching around symptoms.
4. Run the relevant integration/build checks, inspect the actual output, and compare it with each `Expected:` line. Record legitimate deviations with the specification and justification.
5. Commit only this task's owned changes when valid and allowed; record commit, command, status and remaining gaps in the log. Continue to the next task without human check-ins.

If a task changes the approved architecture, undermines acceptance criteria, or requires an important technical exception, route the precise proposed deviation to a **fresh T3 Code reviewer** using `superpowers:t3-model-gates` (attach the complete `t3-approver/SKILL.md`). Record `APPROVE / REVISE / BLOCK` before implementing the deviation. Otherwise routine TDD and tests are sufficient within the task loop.

## Final independent code gate

After all tasks, run the complete verification commands, check the real diff, and capture the base and head commits. Invoke `superpowers:requesting-code-review` to launch a **GPT-6.1 Sol / High T3 thread** with design, plan, review-focus list, relevant commit range, repository references, tests and known limitations. The reviewer inspects read-only and returns an explicit verdict.

- On `REVISE`, reproduce material findings where possible, fix them natively, re-run targeted and complete tests, then ask a **new independent review round** for approval.
- On `BLOCK`, do not mark complete. Resolve the listed blocker or report why it cannot be resolved.
- On `APPROVE`, record the verdict and run `superpowers:finishing-a-development-branch`.

Never replace this gate with a self-review or unverified model. Do not ask the user to review your diff. A model verdict does not authorize destructive or externally visible actions; follow `superpowers:t3-model-gates` boundaries.
