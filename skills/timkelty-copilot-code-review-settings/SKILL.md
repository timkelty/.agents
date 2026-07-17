---
name: copilot-code-review-settings
description: Show the effective GitHub Copilot Code Review settings for a repository and target branch, including automatic reviews, draft pull requests, new pushes, enforcement, and source rulesets. Use when the user invokes `$copilot-code-review-settings`, asks to inspect, show, or verify Copilot Code Review settings for a repository, pull request, or branch, or another skill explicitly requires this check, especially `pr`.
---

# Copilot Code Review Settings

Show the effective repository and inherited ruleset configuration without changing it.

1. Resolve the GitHub repository and target branch. Use an explicitly requested branch when present, an existing pull request's base branch next, and the repository's default branch otherwise.
2. Use `gh api --paginate` to read every page of effective rules for that branch, URL-encoding the branch name. Select rules whose type is `copilot_code_review`; the effective branch-rules endpoint includes inherited rules.
3. Read the referenced ruleset details when available to obtain each source's name, scope, and enforcement status. If multiple effective rules apply, list every source and treat an option as enabled when any active applicable rule enables it.
4. Render this Markdown, replacing the repository, branch, and values:

   ```markdown
   ### [Copilot Code Review Settings](https://github.com/OWNER/REPO/settings/copilot/code_review)

   | Setting | Value |
   |---|---|
   | Automatic reviews | Enabled |
   | Draft pull requests | Enabled |
   | New pushes | Enabled |
   | Enforcement | Active |
   | Target branch | `main` |
   | Source | Organization ruleset: `Copilot Code Review` |
   ```

5. Use `Not configured` when no effective Copilot Code Review rule applies. Use `Unknown` when permissions, authentication, or API availability prevent reading a value, and briefly state why. Do not fail the surrounding workflow.

Report repository and inherited ruleset settings only. Do not infer the user's separate personal automatic-review setting from pull request activity.
