# Agent Selection Rules

Use the right agent for the right task. Some are mandatory.

## Mandatory Agents

| Agent | MUST USE When |
|-------|---------------|
| `planning-architect` | Planning features, refactors, architecture decisions |
| `security-auditor` | Auth, authorization, API security, data handling |
| `test-engineer` | Test strategy, coverage analysis, test automation |

## Recommended Agents

| Agent | Use For |
|-------|---------|
| `implementation-engineer` | Executing approved plans |
| `code-reviewer` | Quality and maintainability review |
| `code-simplifier` | Reducing complexity, cleanup |

## Agent Selection Flow

```
Is it planning/architecture?
  -> YES: planning-architect (Opus)

Does it touch auth/security?
  -> YES: security-auditor (Sonnet)

Is it executing an approved plan?
  -> YES: implementation-engineer (Sonnet)

Does it need simplification?
  -> YES: code-simplifier (Opus)
```

## Model Selection

| Complexity | Model | Agent Types |
|------------|-------|-------------|
| Architecture | Opus | planning-architect, code-simplifier |
| Implementation | Sonnet | implementation-engineer, security-auditor |
| Exploration | Haiku/Sonnet | explorer, codebase-explorer |

## Parallel Execution

When tasks are independent, run agents in parallel:

```
/ship "Add auth feature"
  -> parallel: security-auditor + test-engineer
  -> sequential: implementation-engineer (after tests)
```

## Never Do

```
BLOCKED: Skip planning-architect for complex features
BLOCKED: Skip security-auditor for auth changes
BLOCKED: Use Opus for simple implementation (wasteful)
BLOCKED: Use Haiku for security audits (insufficient)
```
