---
name: create-pr
description: Use when ready to create a pull request. Enforces squash merge, convention compliance, conflict resolution, and the full pre-PR checklist before calling gh pr create.
---

# Create PR

## Overview

Prepare and create a pull request that passes review on the first try. Enforces
squash commits, naming conventions, conflict-free rebasing, and a complete PR
description before touching `gh pr create`.

**Announce at start:** "I'm using the create-pr skill to prepare this PR."

**Core principle:** Fix everything BEFORE creating the PR. A PR that needs
follow-up commits for convention violations or merge conflicts is a failed PR.

## Pre-PR Checklist

Run every check. If ANY fails, fix it before proceeding. Do not skip checks.

### 1. Verify Clean State

```bash
git status
```

- No uncommitted changes (staged or unstaged)
- No untracked files that should be committed
- If dirty: commit or stash before proceeding

### 2. Identify Base Branch

```bash
git merge-base --fork-point main HEAD || git merge-base main HEAD
```

Default base is `main` unless the user specifies otherwise.

### 3. Rebase onto Latest Base

```bash
git fetch origin main
git rebase origin/main
```

- If conflicts arise: resolve them, `git rebase --continue`
- If rebase is messy (many conflicts): ask the user before proceeding
- NEVER create a merge commit -- always rebase

### 4. Squash to Single Commit (Before PR Only)

This step applies ONLY when creating a new PR. If a PR is already open,
skip this -- push follow-up commits instead. GitHub squash merge handles
the final collapse at merge time.

Count commits ahead of base:

```bash
git log --oneline origin/main..HEAD | wc -l
```

If more than 1 commit, squash:

```bash
git reset --soft origin/main
git commit -m "<message>"
```

The commit message must follow conventions (see Step 5).

If exactly 1 commit, verify its message follows conventions.

### 5. Verify Naming Conventions

Check against the project's conventions. Read these if they exist:
- `platform/shared/conventions.md` (issue traceability, naming rules)
- `.claude/rules/commits.md` (commit message format)
- `.claude/rules/pull-requests.md` (PR format)

**Branch name:**
- Format: `{type}/{ID}-{slug}` (e.g., `feat/FQ-42-reward-shop`)
- Types: feat, fix, chore, docs, test
- If branch name is wrong: `git branch -m <correct-name>`

**Commit message:**
- Format: `{type}({ID}): short description` or `{type}: short description`
- First line: max 72 chars, imperative mood
- Body: wrap at 72 chars, explain what and why
- No AI mentions, no Co-Authored-By tags
- If wrong: `git commit --amend` to fix

**PR title:**
- Same format as commit message first line
- Under 72 characters

### 6. Run Tests

```bash
# Detect and run the project's test suite
# Try in order: pnpm test, npm test, swift test, pytest, cargo test
```

- If tests fail: fix before proceeding
- If no test suite exists: note it in the PR description

### 7. Check for Anti-Slop Violations

Scan changed files for violations:

```bash
git diff origin/main..HEAD
```

Check for:
- Unicode violations: em dashes, smart quotes, ellipsis characters
- AI mentions in code comments, commit messages, or docs
- Co-Authored-By tags
- Files that shouldn't be committed (.env, credentials, .DS_Store)

Fix any violations before proceeding.

## PR Creation

### 8. Discover PR Template

```bash
ls .github/PULL_REQUEST_TEMPLATE.md 2>/dev/null
ls .github/pull_request_template.md 2>/dev/null
ls docs/PULL_REQUEST_TEMPLATE.md 2>/dev/null
```

If a template exists, follow it exactly. If not, use the standard format.

### 9. Build PR Description

Write like a human. Summarize what matters, skip what doesn't.

```bash
git diff --stat origin/main..HEAD
```

**Standard format:**

```markdown
## Summary

<2-3 sentences: WHAT changed and WHY>

## Key Changes

- <The important thing that happened>
- <Another important thing>
- <Anything a reviewer should pay attention to>

## Callouts

- <Breaking changes, if any>
- <Decisions worth explaining>
- <Known limitations or follow-up work>

## Test Plan

- [ ] <How to verify these changes work>
```

**Writing the description:**
- Lead with the why, not the what. The diff shows the what.
- Highlight decisions, trade-offs, and anything non-obvious.
- Call out breaking changes, new dependencies, or risky areas.
- Skip routine files (config, generated bundles, boilerplate).
- For large PRs: a paragraph overview, then bullet the major areas.
- Do NOT list every file. A reviewer can read `--stat` themselves.

### 10. Push and Create

```bash
git push -u origin HEAD

gh pr create \
  --title "<type>({ID}): description" \
  --body "$(cat <<'EOF'
<PR body from step 9>
EOF
)"
```

Report the PR URL when done.

## Quick Reference

| Check | What | Fix If Wrong |
|-------|------|-------------|
| Clean state | No uncommitted changes | Commit or stash |
| Rebased | No conflicts with base | `git rebase origin/main` |
| Squashed | Single commit | `git reset --soft origin/main && git commit` |
| Branch name | `{type}/{ID}-{slug}` | `git branch -m <name>` |
| Commit message | `{type}({ID}): desc` | `git commit --amend` |
| Tests pass | Exit code 0 | Fix failures |
| No anti-slop | No unicode, no AI mentions | Fix violations |
| PR template | Followed if exists | Adjust description |
| Description | Highlights what matters, not a file list | Rewrite for humans |

## Red Flags

**Never:**
- Create a PR with merge conflicts
- Skip the test verification step
- Use a PR title that doesn't match conventions
- Include .env, credentials, or .DS_Store in the PR
- Mention AI/Claude in PR title, description, or commits
- Force-push to someone else's branch
- Amend or force-push after the PR is open (push follow-up commits instead)

**Always:**
- Rebase before creating (never merge commits)
- Squash to a single commit
- Run the full checklist even if "it's just docs"
- Write the description for a human reviewer, not a checklist
- Follow the PR template if one exists
