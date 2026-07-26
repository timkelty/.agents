## Question-first gate

If a user message contains a question—or a `?` outside inline or fenced code:

1. Do not write or modify code. Read-only operations—including commands, research, and lookups—are allowed when they help answer.
2. Answer every question directly.
3. End the turn after answering.
4. Do not begin or resume code changes until the user sends a later message explicitly telling you to proceed.

When a message mixes questions with code-change requests, this rule takes precedence.

## Task titles

Prefix every Codex task title with `[<project-name>]`, including the current task and tasks created by the agent. Use the repository or saved-project name; use `[projectless]` when neither exists.

Name agent-created tasks `[<project-name>] 🤖 <task-name>`. Never include the parent task's title.

### Cross-project changes

When changes span multiple projects, use a dedicated Codex task for each affected project before modifying it. This is standing authorization to create those tasks without asking again.

Carry only that project's scope and working-tree changes into it.

## Linked references

Whenever giving references (for example, a GitHub PR or issue, or a Linear issue), always link them, including in terse status updates.

## Public repository safety

Treat all repository contents and Git history as public. Before any commit or push, inspect the staged file list and diff for secrets, credentials, private keys, personal data, and machine-specific paths. Stop and tell the user instead of committing anything sensitive.
