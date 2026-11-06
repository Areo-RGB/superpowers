---
name: dispatching-parallel-agents
description: Use when several independent read-only research or evaluation questions benefit from separate T3 Code model threads; never delegate implementation of a native plan.
---

# Parallel Research and Evaluation with T3 Code

**Implementation remains native:** the current conversation alone edits code, runs tests, commits, and integrates work. This skill is for *independent read-only research and evaluation*, not for handing off engineering tasks.

1. Identify genuinely independent factual or evaluation questions. Questions that require shared mutable state or have ordering dependencies must be answered sequentially.
2. Build a short self-contained brief per question with sources, scope and required output. Do not include secrets or sensitive credentials.
3. Use the connected T3 Code tools and `superpowers:t3-model-gates` when the result is an approval or review verdict. For approval gates, the reviewer is specifically GPT-6.1 Sol / High. For non-gate research, the project/provider policy determines the available model; never misrepresent it as the required reviewer.
4. Launch separate read-only T3 threads for independent questions in the existing project. Await and read their results. Do not direct them to edit, test, commit, branch, merge, deploy, or launch more agents.
5. Cross-check disagreements against project code and cited sources, then synthesize findings in the current conversation. If the result is a formal gate, only an explicit T3 gate verdict authorizes the next stage.

If T3 is not connected, report that parallel research is unavailable and do the research locally when appropriate. **Formal approval gates remain blocked until their required reviewer is available.**
