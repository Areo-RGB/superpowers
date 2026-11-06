---
name: brainstorming
description: Use before new creative or software changes to establish intent, examine existing solutions, choose a design, and obtain independent T3 Code design approval before planning.
---

# Brainstorming

Understand what the user wants, then choose a workable design. **Do not request design approval or design-document review from the user.** The independent GPT-6.1 Sol / High reviewer in `superpowers:t3-model-gates` supplies the design gate; the current agent stays the native implementer.

## Start with context, not permission

Read the user's request, existing repo, relevant artifacts, and current behavior. If the requested result is already clear, do not ask permission to begin. If a product requirement would materially change the deliverable and cannot be learned from context, ask one targeted question, but never turn a routine engineering choice into a human approval gate.

Before building something new, look for a reliable existing library, service, or implementation. Compare reuse and build-from-scratch on fit, cost, security, maintenance, license, and integration constraints. Do not reinvent functionality available in a suitable maintained dependency. Use web/docs/repo research when needed.

## Design flow

1. Capture purpose, audience, successful behavior, constraints, non-goals, and important failure cases. Separate stated requirements from your assumptions.
2. Consider at least two credible approaches where material alternatives exist. Select one based on the user's goals and the existing project; record meaningful tradeoffs. If there is only one reasonable path, explain why.
3. Write the design to `docs/superpowers/specs/YYYY-MM-DD-<topic>-design.md`, unless the project has another convention. A small change gets a short design; a large change includes components, data flow, edge cases, observability, tests, and migration concerns.
4. Load `superpowers:t3-model-gates` and embed the entire `t3-approver/SKILL.md` in the T3 request. Send the design, the original requirements, alternatives, constraints, and project references to a fresh **read-only T3 Code design reviewer**, explicitly configured for GPT-6.1 Sol at High effort.
5. On `REVISE`, investigate and improve the design; get a new reviewer verdict. On `APPROVE`, record the review thread/decision and move directly to `superpowers:writing-plans`. Do not wait for a human sign-off on the doc.

Design reviewer checks: does the proposal really meet the user goal; is there a simpler premade solution; are facts and interfaces grounded; are the edge cases, costs, risks, and acceptance tests sufficient; are assumptions made visible? The reviewer decides design readiness, not the user's unexpressed preferences.

## Interaction and prototypes

Show mockups, text examples, diagrams, or a throwaway spike when they materially improve clarity. Existing `visual-companion.md` is available for a visually demanding design. Do not convert visual feedback into a mandatory approval stop. A spike is not production code; its discoveries feed the design and plan, then the reviewer gates the production approach.

## Do not proceed if

The essential user goal remains unknowable, the reviewer returned `BLOCK`, the model/effort could not be verified, or the artifact still has unresolved critical/important objections. Report the exact obstacle instead of claiming approval.
