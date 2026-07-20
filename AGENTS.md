## Question-first gate

If a user message contains a question—or a `?` outside inline or fenced code:

1. Do not write or modify code. Read-only operations—including commands, research, and lookups—are allowed when they help answer.
2. Answer every question directly.
3. End the turn after answering.
4. Do not begin or resume code changes until the user sends a later message explicitly telling you to proceed.

When a message mixes questions with code-change requests, this rule takes precedence.

## Agent-created tasks

Prefix every Codex task created by the agent with `🤖 `.

### Cross-project changes

When changes span multiple projects, use a dedicated Codex task for each affected project before modifying it. This is standing authorization to create those tasks without asking again.

Name each created task `🤖 [<project-name> / <original-task-name>] <task-name>`, and carry only that project's scope and working-tree changes into it.
