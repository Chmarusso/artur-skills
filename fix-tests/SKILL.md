---
name: fix-tests
description: Run tests stopping at first failure, fix it, repeat until all pass. Can also fetch CI failures from GitHub PR. Shows summary at the end.
user_invocable: true
---

# Fix Tests

Run the test suite iteratively, fixing one failing test at a time until all tests pass.

## Instructions

### 0. Check for CI failures on the current PR (optional first step)

Before running tests locally, check if there's a GitHub PR for the current branch with failed CI checks:

```bash
gh pr checks --fail 2>/dev/null
```

If there are failed checks, fetch the CI logs to identify failures:

```bash
# Get the failed run IDs
gh pr checks --json name,state,link 2>/dev/null | jq '.[] | select(.state == "FAILURE")'

# List recent failed workflow runs for the current branch
BRANCH=$(git branch --show-current)
gh run list --branch "$BRANCH" --status failure --limit 3

# View the failed run logs (use the run ID from above)
gh run view <run-id> --log-failed
```

Read the CI failure output carefully. Use these errors as context — they may reveal the same issues you'll encounter locally, or deployment/build errors that need fixing first.

### 1. Run tests with bail (stop at first failure)

Run the **Playwright E2E tests** with `-x` flag (bail on first failure):

```bash
npx dotenv -e .env.test -- npx playwright test -x 2>&1 | tail -100
```

**Important:** Pipe through `tail -100` to avoid output truncation — Playwright can produce very long output and we only need the failure details at the end.

If the user passed arguments (e.g. a specific test file or grep pattern), include them:

```bash
npx dotenv -e .env.test -- npx playwright test -x <user-args> 2>&1 | tail -100
```

### 2. If all tests pass

Skip to step 5 (summary).

### 3. If a test fails

1. **Read the failure output carefully.** Identify the failing test file, test name, and error message.
2. **Read the relevant test file and source code** to understand what the test expects and why it fails.
3. **Fix the issue.** This could be:
   - A bug in the application source code
   - A bug or outdated assertion in the test itself
   - A missing migration, fixture, or setup step
4. **Do NOT skip, delete, or disable tests.** The goal is to make them pass, not to remove them.

### 4. Re-run tests

Go back to step 1. Continue this loop until all tests pass.

### 5. Summary

After all tests pass, print a summary:

```
## Fix Tests Summary

- **Total iterations:** <number of test runs>
- **CI failures found:** <yes/no, brief description if yes>
- **Fixes applied:**
  1. <file>: <brief description of what was wrong and how it was fixed>
  2. ...
- **Result:** All tests passing
```
