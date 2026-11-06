---
name: finishing-a-development-branch
description: Use when implementation is tested and independently reviewed, to record a safe finish decision without requesting routine human review or approval.
---

# Finishing a Development Branch

Finish in the **existing checkout**. Do not offer a workflow selection menu and do not change or delete unrelated git state.

## Finish checklist

1. Run the full relevant test/build/lint checks on the current commit; report concrete commands and observed results. Do not claim success if any required check failed or was skipped.
2. Confirm `git status --short`, current branch, intended base branch (from project config, plan, history or upstream), and final committed diff. Never guess a merge target.
3. Check that `superpowers:requesting-code-review` returned a verified **GPT-6.1 Sol / High** `APPROVE` verdict covering the current revision. A previous verdict on a different diff does not qualify.
4. Present the final evidence and your proposed safe disposition to a fresh T3 Code **finish reviewer** with `superpowers:t3-model-gates` (send the full `t3-approver/SKILL.md` in the approval request): completion vs missing work, tests, remaining issues, git status, and intent to retain the branch locally. Execute only after `APPROVE`.
5. **Default disposition:** leave the completed changes committed on the existing branch, with all files and history preserved; provide a concise summary, test results, review verdict and remaining concerns. No integration menu or user sign-off step.

## Explicit integration requests

If the user already requested a local merge or a pull request, follow that instruction **after** checking correct destination, green tests, clean relevant state, and T3 finish review. Use the provider's established tooling and report the actual outcome. Do not invent remote URLs. Never infer a request to push or publish from a generic request to build or finish.

An independent reviewer makes engineering judgments, **not access decisions**. Do not auto-push, deploy, force-push, discard work, destroy branches or override tool permissions merely because the reviewer approved. If a remote/public/destructive action needs platform permission or explicit user authorization, leave the action pending and say why. A failed merge/test halts integration; preserve recoverable state.

## Completion report

Include what changed, which checks ran and their outcomes, the current branch and commit, T3 review thread/verdict, and any deferred issues. No question asking the user to review the work or choose a post-development menu.
