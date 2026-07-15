---
name: pr
description: Get the user's current changes or existing pull request ready for human review, including rebasing, draft PR setup, review follow-up, and CI or conflict handling. Use when the user invokes `$pr` or asks to prepare, revive, rebase, babysit, or finish a pull request.
---

# PR

Get the current changes into a pull request ready for human review.

Keep orchestration, review-feedback fixes, commits, pushes, review-thread replies and resolution, and the readiness decision in this task. Separate agents may perform read-only reviews only. Native PR Auto-fix is the sole exception and may repair CI failures and conflicts under step 9.

1. Identify the branch containing the changes and whether that branch already has an open pull request. If the changes are not on a suitable branch, create one.
2. Determine the base branch. Use the existing pull request's base when present; otherwise use the repository's default branch unless the user specified another.
3. If the `copilot-code-review-settings` skill is available, invoke it for the chosen base branch before changing the pull request's draft state. If it is unavailable, continue and report that the settings check was skipped.
4. Fetch the latest base branch and, when it exists remotely, the working branch. Before rewriting history, compare the local head with its remote-tracking branch. If the remote head contains commits absent locally, stop and ask rather than overwriting them. Rebase onto the remote base branch while preserving uncommitted changes. If updating the remote requires a force push, use `--force-with-lease` and stop if the lease fails.
5. Commit the intended changes and push the branch, leaving unrelated working-tree changes untouched.
6. If the branch has an open pull request, reuse it. If that pull request is not a draft, ask whether to convert it to draft. If no open pull request exists, create a draft pull request targeting the chosen base branch. In either case, ensure its title and description follow the repository's template and accurately describe the current diff and relevant context.
7. Once the pull request exists, associate the task with its head branch when the host supports that operation; do not claim association otherwise. Always provide a clickable link immediately and in the final result. On Codex Desktop, emit the native pull-request directive in the final result only if the pull request was created during this task; do not emit it for an existing pull request.
8. Repeat this review loop against one recorded head commit until every required lane is clean on the same current head:
   - Record the current head commit.
   - Run the host's native read-only correctness review against the base. If none is available, perform a fresh correctness review of the diff.
   - If Ponytail review is available, run it against the same diff. If it is unavailable, continue without it and report the skip in the final result; do not treat its absence as a failed gate.
   - Ensure Copilot Code Review evaluates the recorded head. Rely on automatic review only when the reported settings cover the pull request's current draft state and new pushes; otherwise explicitly request `copilot-pull-request-reviewer[bot]`. If a current-head review cannot be requested or observed, ask the user instead of waiting indefinitely and leave the pull request draft.
   - Ask the user when reviewers disagree or a finding would change scope or product behavior.
   - Address actionable, in-scope findings; validate; commit; and push. Prefix agent-written review-thread replies with `🤖 Agent response:`. Reply before resolving a review thread, and resolve only addressed or obsolete threads.
   - If a commit or push changes the head, restart the entire loop. Exit only when the recorded head is still current and has no unresolved actionable findings.
9. Use native PR Auto-fix when available for recurring check failures and merge conflicts. Auto-fix may repair the branch in a separate task; it is not a subagent. This task remains the coordinator and sole owner of the readiness decision. While Auto-fix is active, do not make concurrent mutations here. Use the host's wait or resume mechanism to return here after Auto-fix pauses, then re-read live PR state and restart the full review loop if the head changed. If Auto-fix cannot be enabled or this task cannot resume afterward, handle PR-caused failures and conflicts here instead. Never run both repair paths concurrently.
10. Immediately before readiness, fetch the remote base again. If its tip advanced, rebase, push, and restart the review and check loops. Refresh the pull request title and description to match the final diff and validation. Mark the pull request ready for human review only when it is rebased, mergeable, scoped correctly, validated, all required checks pass, and every required review lane is clean on the same current head. Otherwise leave it draft and state the exact remaining gate.
11. Finish with the pull request link and a short summary of validation and review status.

Do not merge, release, delete branches, broaden scope, or resolve unanswered feedback.
