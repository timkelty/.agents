---
name: agentify
description: Turn an observed agent behavior problem or preference into the smallest durable improvement without derailing the current task. Use when the user wants future agents to behave differently, asks whether guidance belongs in native settings, instructions, memory, a skill, or a plugin, asks to preserve a workflow preference, or invokes $agentify.
---

# Agentify

Fix recurring agent behavior at its source, not one response at a time.

1. State the trigger and desired future behavior in one concrete sentence. Preserve the user's terminology when they provide exact wording.
2. Inspect the instructions and native features already governing the behavior, then check installed skills or plugins when relevant. Prefer an existing mechanism that fully solves the problem. Search externally only when current product behavior or an established third-party solution matters.
3. Choose the smallest durable home:
   - Instructions for standing rules that should always apply in their scope.
   - Memory for Codex-specific preferences or project lessons, only when the user explicitly asks to remember or update them.
   - A skill for a repeatable workflow or specialized procedure.
   - A plugin or app for integration, distribution, external state, or lifecycle behavior.
   - Nothing when the issue is one-off or already covered.
4. Before editing, identify the source of truth. Do not patch generated files, plugin caches, installed copies, or other overwrite-prone artifacts when an owned source exists.
5. Apply one focused change in one place when possible. Do not duplicate guidance across homes unless each copy serves a distinct scope.
6. Validate the changed mechanism and report the captured behavior, its durable home, files changed, and validation performed.

Keep the current task moving. Do not turn a small behavior refinement into a broad research project or unrelated cleanup.
