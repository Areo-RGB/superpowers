---
name: t3-model-gates
description: Use for every Superpowers design, plan, implementation, review, exception, or finish gate; route judgment to an independent GPT-6.1 Sol reviewer at High effort in T3 Code instead of requesting human sign-off.
---

# T3 Code Model Gates

The implementing agent works **natively in its current conversation**. It is responsible for planning, editing, testing, committing, and reporting. A **separate read-only T3 Code thread** running **GPT-6.1 Sol / High** makes independent gate decisions. Every approval request **must carry the complete `t3-approver/SKILL.md` instructions** to that thread; a file path or skill name alone is not sufficient. Never ask the user to approve or review routine design documents, plans, diffs, exception rulings, or task completion. Never substitute the implementing agent's own approval for the independent verdict.

## Connect to the real T3 Code environment

1. Discover T3 Code's MCP tools through the environment's MCP connection, not by guessing tool names or arguments. The owner can get the endpoint from **Settings → Environments → Copy MCP URL** (typically `https://<environment-address>/mcp`). An external client must be connected through the supported OAuth flow with permission to start and message threads. An existing authenticated connection requires no extra setup.
2. Call `orchestrator_capabilities` to inspect provider instances, models, and model options. For an ordinary independent thread, use `t3_thread_launch`; use `t3_thread_wait` and `t3_thread_read` to collect the verdict. `t3_thread_configure` can read or change the review thread's model options. `t3_thread_send` is for follow-up questions in that same review thread. These are genuine T3 MCP tools, not shell commands.
3. Resolve the actual Codex provider **instance ID** from the capability listing. Select the catalog model `gpt-6.1-sol` (label may read `GPT-6.1 Sol`) and its advertised **High** reasoning-effort option. Do not assume a model string alone sets thinking effort. `ModelSelection` contains an `instanceId`, `model`, and provider-specific `options`; use the *live option descriptors/schema* instead of inventing an `options` structure.
4. Launch the reviewer in the **same existing project checkout**, using the default/root checkout binding and an explicit review-only prompt. Do not create a second checkout or change any branch. Read its configuration using `t3_thread_configure` or the thread metadata. Confirm both the model and High effort actually took effect. If not, correct the setting before accepting the review.
5. `t3_thread_wait` can time out without cancelling; follow with `t3_thread_read` and wait again if necessary. A completed run is not by itself an approval. Extract an explicit verdict with evidence. Keep a link or thread ID and the reviewer response in the project decision log.

T3's local CLI `t3 thread start`, `t3 thread send --wait`, and `t3 thread show` can run threads using project defaults, **but do not by themselves guarantee the required model and High effort**. Use the MCP model configuration or explicitly verify and set the model in the T3 UI before using CLI review output. Prefer MCP.

## Send the approver skill with every approval request

The separate review model must **receive the actual approver skill instructions together with the request**, even when its own environment also has the skill installed.

1. Read the complete current file at **`/home/paul/Projects/my-skills/skills/t3-approver/SKILL.md`**. This is the intended install location. If the external file does not exist or cannot be read, use the bundled **`skills/t3-approver/SKILL.md`** from the Superpowers installation. Do not use a cached or summarized version. If neither file is available, block the gate rather than sending an ungoverned reviewer prompt.
2. Construct the **single actual `message` supplied to `t3_thread_launch`** as two ordered parts. The first part is `APPROVER_SKILL_MD` containing the **verbatim entire contents** of that skill file (frontmatter included). The second is `APPROVAL_REQUEST` with the gate name, user's requirements, affected artifact/revision, evidence, question, and required verdict. Delimit the parts clearly, for example:

   ```text
   BEGIN APPROVER_SKILL_MD
   <verbatim complete SKILL.md contents>
   END APPROVER_SKILL_MD

   BEGIN APPROVAL_REQUEST
   <gate and self-contained artifact/evidence to review>
   END APPROVAL_REQUEST
   ```

3. At **each subsequent approval request**, including a new review round or an adjudication through `t3_thread_send`, send the complete current approver skill text together with the updated request. Do not rely on previous thread memory, automatic skill discovery, a filesystem path alone, or a request such as "use the approver skill."
4. The approver skill governs the reviewer's read-only behavior, gate-specific checks, and `VERDICT` report. The original user requirements remain the authority for the product. Keep the submitted evidence accurate and do not put unrelated secrets in review prompts.
5. Verify that the request reached the **independent** thread and inspect the returned verdict. Do not count a reviewer response as valid if the skill was omitted or only referenced instead of actually included.

## Gate protocol

Launch **one new independent reviewer thread for each major gate** (design, plan, final code, finish). A retry or clarification can continue in that thread with `t3_thread_send`; material changes after a rejection require a new review round. Keep the executor's current conversation as the only implementation context.

After the **full approver skill** has been embedded as above, send a self-contained approval request with:

- Gate type and decision requested (design / plan / code / finish / technical exception).
- User's actual objective and binding constraints, with pointers to the relevant spec/plan/repository files.
- Proposed artifact or exact file paths; for code, include base and head revisions, commands and test outcomes, and any known failures or unresolved defects.
- Criteria: correctness, completeness, ambiguity, regressions, security, performance, user experience, and enforceable constraints.
- Instruction: obey the attached `t3-approver/SKILL.md`; **read-only** inspection only; no code edits, commits, branch moves, pushes, external publication, or further agent launches; no human approval requests.
- Required result shape:

```text
VERDICT: APPROVE | REVISE | BLOCK
GATE: design | plan | code | technical-exception | finish
ARTIFACT: <exact artifact/revision reviewed>
REASON: <clear one-paragraph rationale>
FINDINGS:
- [critical|important|minor] <file/line or section, evidence, impact, precise fix>
ASSUMPTIONS: <any material unverified assumptions>
RECHECK: <conditions the executor must verify to proceed>
```

If the reviewer returns no explicit verdict, disagrees with itself, is unable to inspect required evidence, uses an unverified model/effort, or the T3 tools are unavailable, **the gate remains blocked**. Retry tool/model discovery or collect the missing information; if impossible, report the blocker and the work completed. Do not silently switch to another model, self-review, or route the routine gate back to the user.

For `REVISE`, independently validate each finding against the source and tests, make justified fixes, verify them, then submit the revised artifact for a **new reviewer verdict**. For an unsupported review finding, send a technical rebuttal with evidence and ask the reviewer to adjudicate; record the result. `BLOCK` means halt the gated step and report the blocking reason. Record the exact decision and constraints in `docs/superpowers/review-log.md` (or the project's preferred audit location).

## What an approval means (and does not mean)

The reviewer approves the **engineering decision/artifact**, not access rights. A model verdict never grants permissions to install credentials, change account security, disclose secrets, publish data, deploy to production, run destructive commands, force-push, delete unowned branches, override environment policy, or perform any other action requiring the user's authority or the platform's own approval UI. Respect existing tool permissions and explicit user instructions. If such an action is not authorized, leave it undone, report the precise reason, and continue with independently safe work. Distinguish *missing task requirements* (ask the user only when truly essential) from *routine review gates* (T3 reviewer only).

## Authoritative T3 Code references

- https://github.com/pingdotgg/t3code/blob/main/docs/user/outside-agents.md
- https://github.com/pingdotgg/t3code/blob/main/docs/orchestration-v2/orchestrator-mcp-server.md
- https://github.com/pingdotgg/t3code/blob/main/apps/server/src/mcp/toolkits/orchestrator/tools.ts
- https://github.com/pingdotgg/t3code/blob/main/apps/server/src/mcp/toolkits/thread/tools.ts
- https://github.com/pingdotgg/t3code/blob/main/docs/user/permission-modes.md

These are references, not permission to assume any installed version exposes every tool. Inspect the connected environment's current capabilities.
