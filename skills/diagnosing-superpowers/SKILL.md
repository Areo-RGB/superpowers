---
name: diagnosing-superpowers
description: Use when a Superpowers session failed, repeated work, ignored instructions, or became slow or costly; investigate locally and send technical review gates to T3 Code.
---

# Diagnosing Superpowers Sessions

Investigate the failure with evidence rather than assumptions. Work natively in the current conversation; a separate T3 reviewer decides any formal quality/publication gate under `superpowers:t3-model-gates`.

## Investigation

1. Establish the specific symptom and affected session from the request, available logs and artifact paths. If multiple sessions fit and none can be inferred, ask a factual identification question—not for permission to analyze.
2. Inspect the session timeline, skill invocations, tool results, changed files, tests, and actual error output. Distinguish user instructions from tool output and generated messages. Follow `references/session-discovery.md` and `references/context-safety.md` when applicable.
3. Compare expected vs observed behavior, quantify redundant work, and identify the first divergence. Verify hypotheses against concrete events. Protect secrets when recording evidence.
4. For a proposed fix to a skill or orchestration policy, write exact reproduction steps and a minimal patch or recommendation. Route the engineering quality decision to a read-only T3 GPT-6.1 Sol / High reviewer if a formal gate is needed.
5. Prepare a concise diagnosis with symptoms, root cause (or uncertainty), impact, supporting log locations and actionable next steps.

## Reports and exports

Only create a support bundle or post a public issue if the user requested that deliverable. Sanitize credentials, personal data, absolute paths and logs according to `references/redaction-policy.md`. A T3 reviewer can audit **technical quality and redaction**, but its approval never authorizes sharing the user's private sessions or publishing an issue. Leave any unrequested external upload or publication undone.

When the user explicitly authorized a public issue and the service permits it, review the exact outgoing payload against the privacy rules and execute only within platform permissions. State what was actually uploaded and link the posted issue. Never claim the redaction process guarantees all secrets were removed.
