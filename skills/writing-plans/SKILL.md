---
name: writing-plans
description: Use when a design is ready to turn into a checkable native implementation plan; the plan gate is performed by a separate GPT-6.1 Sol High T3 Code reviewer.
---

# Writing Implementation Plans

Create a plan that the current agent can execute **natively in this conversation**. There is no execution-method menu, task-implementer delegation, or human plan-approval request.

**Plan location:** `docs/superpowers/plans/YYYY-MM-DD-<feature>.md` unless the project requires a different path.

## Plan construction

1. Read the design and its independent T3 design verdict. Inspect existing code, dependencies, tests and naming conventions; do not invent APIs or file paths.
2. Record the goal, exact spec path, architecture, technology constraints, package or dependency choices, and non-goals.
3. Break work into testable tasks small enough to complete and commit coherently. Each has exact files, interface signatures, expected behavior, test names and commands. Include migrations, error paths, and compatibility as applicable.
4. Within each task use RED–GREEN–REFACTOR steps: write a failing test; run it and observe the intended failure; implement minimal behavior; run tests; commit when green. For non-code work give an equivalent observable verification.
5. Include a `Review Focus` section listing likely bugs or user-visible inputs that the obvious tests do not cover, plus acceptance criteria and a final full-suite command.
6. Check consistency across task interfaces, spec coverage, ordering, reproducibility, missing file references, and proportion. Write a direct implementation plan rather than duplicating full source files.

Suggested structure:

```markdown
# <Feature> Implementation Plan
**Goal:** ...
**Spec:** docs/superpowers/specs/...-design.md
**Architecture:** ...
**Constraints:** ...
**Review Focus:** ...

## Task 1: <observable deliverable>
**Files:** exact/paths
**Interfaces:** input/output signatures and dependencies
- [ ] Write the failing test: <path and assertions>
- [ ] Run: <command>; Expected: <specific failure>
- [ ] Implement: <exact interface and intended behavior>
- [ ] Run: <command>; Expected: PASS
- [ ] Commit: <message>

## Final verification
<full test/build/static checks>
```

## Independent plan gate

Load `superpowers:t3-model-gates`, include the complete `t3-approver/SKILL.md` with the approval request, then launch a separate **read-only GPT-6.1 Sol / High** T3 Code thread. Give it the plan, design, original request, affected files and exact acceptance criteria. Ask for `APPROVE / REVISE / BLOCK` for correctness, feasibility, dependency ordering, test strength, scope, premade solutions, and neglected risks. Address material findings and re-submit until an explicit `APPROVE`. Record the verdict and thread link.

On approval, **immediately invoke `superpowers:executing-plans`** in the current conversation and work in the existing checkout. Do not ask the user to read/approve the plan or pick an implementation approach. An unknown requirement may justify a narrowly scoped question; that is not a review gate.
