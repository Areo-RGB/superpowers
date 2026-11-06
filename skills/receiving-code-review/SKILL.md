---
name: receiving-code-review
description: Use to validate and act on independent T3 Code reviewer findings without asking the user to arbitrate ordinary technical review decisions.
---

# Receiving Code Review

Apply technical judgment to independent review findings. The reviewer is the configured **GPT-6.1 Sol / High** T3 Code thread from `superpowers:t3-model-gates`, not the current implementing agent.

1. Read the complete findings and identify their evidence, affected file and expected behavior.
2. Check each claim against the actual code, specification, supported environments and tests. Do not agree reflexively or blindly apply large refactors.
3. Reproduce critical and important defects with a failing test wherever practical. Fix them **natively in this conversation**; run relevant checks after each fix.
4. If the finding contradicts a user requirement or an established design, prepare an evidence-based rebuttal and submit it to the same reviewer through `t3_thread_send`, or launch a new gate round for an amended design. Record its verdict. Do not silently override explicit constraints.
5. After changes, re-run complete verification and obtain a new independent T3 review verdict for the updated artifact. Retain severity, decision, and deferral rationale in `docs/superpowers/review-log.md`.

For unclear findings, request clarification **from the T3 review thread**, not a routine human review meeting. Ask the user only when a genuinely missing business requirement or real-world authority cannot be inferred. If a tool or policy requires actual human permission, respect it; model judgment cannot waive it.

**Outputs:** succinct list of accepted/fixed findings, rejected suggestions with evidence, deferred minor issues, test results, and the final reviewer verdict/thread link. Avoid performative agreement.
