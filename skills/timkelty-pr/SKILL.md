---
name: pr
description: Get the user's current changes or existing pull request ready for human review, including rebasing, draft PR setup, review follow-up, and CI or conflict handling. Use when the user invokes `$pr` or asks to prepare, revive, rebase, babysit, or finish a pull request.
---

# PR

Get the current changes into a pull request ready for human review.

Own every mutation and the readiness decision in this task. Separate agents may perform read-only reviews only.

1. Identify the branch, any existing pull request, and the base branch. Create a suitable branch when needed. Use the existing pull request's base when present; otherwise use the repository default unless the user specified another. Keep the pull request draft throughout the agent workflow; convert an existing ready pull request to draft before continuing.
2. Fetch the latest base branch and, when it exists remotely, the working branch. Before rewriting history, compare the local head with its remote-tracking branch. If the remote head contains commits absent locally, stop and ask rather than overwriting them. Rebase onto the remote base while preserving uncommitted changes. Use `--force-with-lease` when the rebase requires a force push, and stop if the lease fails.
3. Inspect the staged, unstaged, and untracked changes before committing. Stage only the intended paths; stop and ask if unrelated staged changes cannot be safely excluded. Run the relevant local validation, commit local changes when present, and push any unpushed commits.
4. Reuse the existing pull request or create a draft pull request against the chosen base. Keep the title synchronized with the diff and default the description to one or two short, outcome-focused sentences. Link related artifacts and preserve required template content. Provide the pull request link immediately and in the final result.
5. Repeat this settle loop against one recorded head until every available gate is clean on that same head:
   - Record the current head, then verify the branch is rebased, the pull request is mergeable, and every required check passes. Investigate and fix pull-request-caused failures or conflicts in this task; report baseline, external, or unavailable failures as the exact remaining gate. If the host explicitly offers PR Auto-fix, do not mutate concurrently; re-read live state after it pauses and restart if the head changed.
   - Run the host's native read-only correctness review against the base, or perform a fresh review of the diff when unavailable. Run Ponytail review against the same diff when available; otherwise report that skip without failing the gate.
   - Treat every expected automated reviewer as a required lane when repository configuration, pull request history, or host behavior indicates it should cover the current state and head. Use each reviewer's normal trigger; do not request it manually unless that is its documented contract. A lane is settled when its current-head review completes or it is explicitly unavailable, unconfigured, or inapplicable. Wait using the host's bounded resume mechanism. If an expected review has not arrived before yielding, schedule a thread wakeup or heartbeat when available; otherwise report it as the exact remaining gate. Re-read live state on resume.
   - Ask the user when reviewers disagree or a finding would change scope or product behavior.
   - Address actionable in-scope findings, rerun validation, and commit and push any fixes. Prefix agent-written review-thread replies with `🤖 Agent response:`. Reply before resolving a thread, and resolve only addressed or obsolete threads.
   - Restart the loop whenever the head changes. Exit only while the recorded head remains current with no unresolved actionable findings.
6. When every draft gate is clean, rewrite the title and description from the final diff using step 4, re-read the metadata, and ask whether to mark the pull request ready for human review. If the user declines, leave it draft and finish with the link plus validation, check, and review status. If the user approves, fetch the remote base again; if it advanced, rebase, push, restart step 5, and ask again after the new head settles. Otherwise mark the pull request ready, refresh live checks and reviews, and wait for lanes triggered by readiness. Address any failure or feedback before finishing. If a fix changes the head, return the pull request to draft, restart step 5, and ask again when every gate is clean. Use a bounded wait or heartbeat for pending results, and finish only when every gate is clean on the same current head.

Do not merge, release, delete branches, broaden scope, or resolve unanswered feedback.
