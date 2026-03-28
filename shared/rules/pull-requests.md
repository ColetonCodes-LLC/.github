# Pull Request Rules

Create PRs that tell the story of your changes.

## Template Discovery

**BEFORE creating a PR, check for existing templates:**

```bash
ls .github/PULL_REQUEST_TEMPLATE.md
ls .github/pull_request_template.md
ls docs/PULL_REQUEST_TEMPLATE.md
ls .github/PULL_REQUEST_TEMPLATE/
```

**If template exists**: Follow it exactly
**If no template exists**: Use the standard format below

## Standard PR Format

```markdown
## Summary

<2-3 sentences describing WHAT changed and WHY>

## Changes

### <Category 1>
- `path/to/file.ts` - <what changed>
- `path/to/other.ts` - <what changed>

### <Category 2>
- `path/to/another.ts` - <what changed>

## Test Plan

<How to verify these changes work>
```

## File-Based Summary (MANDATORY)

**Every PR must document ALL changed files.** Use `git diff --stat` to
identify files, then group logically by feature, type, or layer.

## Title Guidelines

```
<type>: <concise description under 72 chars>

GOOD: feat: add user preference management
GOOD: fix: resolve session timeout on mobile browsers

BAD: Update files (too vague)
BAD: WIP (never merge WIP PRs)
```

## Large PR Handling

For PRs with 20+ files, add a high-level overview with scope and major
additions before the detailed file listing.

## Review Readiness

Before submitting, verify:

- [ ] Title follows conventional format
- [ ] All changed files are documented
- [ ] Changes are grouped logically
- [ ] Test plan is clear
- [ ] Template is followed (if exists)
