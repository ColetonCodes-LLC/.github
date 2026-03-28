# Security Rules

Security is non-negotiable. These rules are BLOCKING.

## Mandatory Security Auditor

**MUST invoke `security-auditor` agent for:**

- Authentication/authorization changes
- API endpoint creation/modification
- User input handling
- Database queries
- Payment processing
- Session management
- Token handling
- Permission/role changes

```
BLOCKED: Auth changes without security audit
BLOCKED: Skipping OWASP checks
BLOCKED: Hardcoded secrets
```

## OWASP Top 10 Checks

Every security review checks:

1. **Injection** -- SQL, NoSQL, LDAP, OS command
2. **Broken Auth** -- Session management, credential storage
3. **Sensitive Data** -- Encryption, exposure
4. **XXE** -- XML External Entities
5. **Access Control** -- Authorization bypass
6. **Misconfiguration** -- Default settings, verbose errors
7. **XSS** -- Reflected, stored, DOM-based
8. **Insecure Deserialization** -- Object tampering
9. **Vulnerable Components** -- Outdated dependencies
10. **Logging Gaps** -- Insufficient monitoring

## Never Do

```typescript
// NEVER: Hardcoded secrets
const API_KEY = "sk-1234567890";

// NEVER: SQL concatenation
const query = `SELECT * FROM users WHERE id = ${userId}`;

// NEVER: Unvalidated redirects
res.redirect(req.query.returnUrl);

// NEVER: Expose stack traces
catch (err) { res.json({ error: err.stack }); }
```

## Always Do

```typescript
// ALWAYS: Environment variables
const API_KEY = process.env.API_KEY;

// ALWAYS: Parameterized queries
const query = `SELECT * FROM users WHERE id = $1`;

// ALWAYS: Validate redirects
const allowedUrls = ['/dashboard', '/profile'];
if (allowedUrls.includes(returnUrl)) redirect(returnUrl);

// ALWAYS: Safe error messages
catch (err) {
  logger.error(err);
  res.json({ error: 'An error occurred' });
}
```

## Secrets Management

- Use environment variables
- Never commit `.env` files
- Use secret managers for production
- Rotate secrets regularly
