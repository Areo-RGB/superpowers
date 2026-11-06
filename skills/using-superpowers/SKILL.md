---
name: using-superpowers
description: Use at the start of a task to choose the applicable Superpowers skills and enforce native execution with independent T3 Code model gates.
---

# Using Superpowers

Before nontrivial work, inspect the relevant skills and project instructions. Read the **current** skill files rather than relying on memory. Follow applicable process skills, then the task-specific implementation skills.

## Default routing

- New feature or design: `superpowers:brainstorming` → `superpowers:writing-plans` → `superpowers:executing-plans` → `superpowers:finishing-a-development-branch`.
- Debugging: `superpowers:systematic-debugging`, `superpowers:test-driven-development` and `superpowers:verification-before-completion`; if a plan is needed, use the design/plan sequence above.
- Code review: `superpowers:requesting-code-review` and `superpowers:receiving-code-review`.
- Every engineering approval or review checkpoint: `superpowers:t3-model-gates` (include the **full** `t3-approver/SKILL.md` in each T3 approval-request message).

**Execution is native only.** The current agent implements each task in this session and existing checkout. The external T3 Code reviewer evaluates and votes on gates; it never implements in your place. Do not offer execution modes or a workspace-selection menu.

**Review gates are model-driven:** never ask the user to approve a design doc, review a plan, sign off a diff, or pick a routine integration option. Use a separate verified T3 GPT-6.1 Sol / High reviewer thread, fix objections, and continue. The user's explicit requirements take priority over skill defaults; ask narrowly targeted questions only for genuinely unknowable product needs. Actual tool/platform permission and external-action boundaries still apply.

If an applicable skill cannot run because a required T3 tool/model is absent, state the blocked gate accurately instead of claiming the workflow passed.
