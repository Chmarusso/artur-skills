---
name: apply-code-review
description: Fetch PR review comments from GitHub for the current branch and resolve them by applying the suggested changes.
user_invocable: true
---

# Apply Code Review

Fetch all review comments from the GitHub pull request associated with the current branch, then resolve them by applying the suggested changes to the codebase.

## Instructions

1. **Determine the current branch and find its PR:**
   - Run `git branch --show-current` to get the current branch name.
   - Run `gh pr view --json number,title,url,reviewDecision` to get the PR for this branch.
   - If no PR exists, tell the user and stop.

2. **Fetch all review comments:**
   - Run `gh api repos/{owner}/{repo}/pulls/{number}/comments` to get inline review comments.
   - Run `gh api repos/{owner}/{repo}/pulls/{number}/reviews` to get top-level review comments.
   - Also run `gh api repos/{owner}/{repo}/issues/{number}/comments` to get general PR conversation comments.
   - Filter to only unresolved/actionable comments — skip comments that are just approvals, acknowledgments, or "LGTM".

3. **Present a summary to the user:**
   - List each actionable comment with: the file path, line number (if inline), the reviewer, and the comment body.
   - If a comment includes a GitHub suggested change (```suggestion block), note that explicitly.
   - Ask the user to confirm before proceeding with changes.

4. **Apply changes for each comment:**
   - For inline comments with GitHub suggestion blocks: apply the exact suggested code change.
   - For inline comments without suggestions: read the referenced file, understand the reviewer's intent, and implement the fix.
   - For general comments requesting changes: identify the relevant files and make the requested changes.
   - Skip comments that are questions, discussions, or don't require code changes — list these as "skipped" with the reason.

5. **After applying all changes:**
   - Show a summary of what was changed and what was skipped.
   - Do NOT commit automatically — let the user review the changes first.

## Important

- Always read the file before editing it.
- Be conservative — if a comment is ambiguous, skip it and flag it for the user rather than guessing.
- Preserve the existing code style and conventions.
- If a suggested change would break something (type errors, missing imports), fix the breakage as part of applying the suggestion.
