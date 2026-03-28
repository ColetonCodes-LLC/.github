# Commit Rules

Standards for git commits in this project.

## Commit Message Format

Use conventional commits with these types:

| Type | When to Use |
|------|-------------|
| `feat:` | New feature |
| `fix:` | Bug fix |
| `docs:` | Documentation only |
| `refactor:` | Code change that neither fixes nor adds |
| `test:` | Adding or updating tests |
| `chore:` | Maintenance, dependencies, tooling |
| `perf:` | Performance improvement |

## Message Structure

```
<type>: <short description>

<optional body explaining what and why>
```

## Rules

1. **First line**: Max 72 characters, imperative mood ("add" not "added")
2. **Body**: Wrap at 72 characters, explain what and why not how
3. **No AI mentions**: See anti-slop.md -- never reference AI in messages
4. **No Co-Authored-By tags**: Code stands on its own merit. AI attribution
   in commit history contradicts anti-slop principles.
5. **Author matching**: Use the git author configured for the current repo.
   Respect per-repo git config (user.name, user.email) and `gh auth` status.

## Examples

```
feat: add user authentication with Supabase

Implements JWT-based auth flow with:
- Login/logout endpoints
- Session persistence
- Role-based access control
```

```
fix: prevent memory leak in event listeners

Event listeners were not cleaned up when component unmounted.
Added cleanup function to useEffect return.
```

## Git Identity

- Multi-account support: respect per-repo git config
- Never set `git config --global user.name/email` from scripts
- Commits use the author configured for the current repository
