---
name: requesting-code-review
description: Use for substantial implementation diffs, technical exceptions, and before finishing; conduct read-only code review in a separate verified GPT-6.1 Sol High T3 Code thread.
---

# Requesting Code Review via T3 Code

Review is an **independent model gate**, not a human sign-off step. Load `superpowers:t3-model-gates` before starting. **Every T3 code-approval request must include the complete `t3-approver/SKILL.md` contents** in the reviewer message, not only a path or skill name.

## Prepare a reproducible review brief

- Capture `BASE_SHA` from the task's recorded starting commit or the confirmed branch base and `HEAD_SHA` using `git rev-parse HEAD`; never guess `HEAD~1` for multi-commit changes.
- Give the reviewer the design, implementation plan, user requirements, `Review Focus`, target files, test commands/actual output, migrations and known risks. Provide the exact diff range (`git diff BASE_SHA..HEAD_SHA`) and working-tree modifications not included in HEAD.
- Include `skills/requesting-code-review/code-reviewer.md` as a checklist if available, but the **current skill** defines the actual T3 launch and verdict protocol.

## Reviewer execution

1. Discover T3 tools/capabilities and configure the separate Codex model `gpt-6.1-sol` at the advertised High effort. Use `t3_thread_launch` in the existing project with a **read-only reviewer-only prompt**.
2. Wait with `t3_thread_wait`; use `t3_thread_read` to collect the full reasoning summary and explicit structured `APPROVE / REVISE / BLOCK` verdict. Verify the thread's actual model and effort. Record its thread link/ID.
3. Assess findings technically, not ceremonially. Reproduce important bugs. Fix blocking defects with tests, and submit the updated revision for a fresh review decision. Minor suggestions can be logged as deferred with reason.
4. Never submit an unreviewed final diff as fully approved. If the T3 review is inaccessible or not on the correct model/effort, report the gate as blocked rather than performing it yourself or asking for routine user review.

A final approval applies only to the exact artifact/revision the reviewer inspected. Repeat the review if material code changes after approval.
