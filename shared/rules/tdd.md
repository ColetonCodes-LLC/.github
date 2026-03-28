# TDD Rules

Test-Driven Development is mandatory for all implementation.

## The Red-Green-Refactor Cycle

```
1. RED    -> Write test that FAILS
2. GREEN  -> Write minimum code to PASS
3. REFACTOR -> Clean up while keeping GREEN
```

## BLOCKING Rules

**Implementation is BLOCKED until:**

1. Tests exist for the feature
2. Tests are currently FAILING (red phase)
3. Tests define expected behavior

```
BLOCKED: Writing code before tests exist
BLOCKED: Tests that pass before implementation (test is broken)
BLOCKED: Modifying tests during green phase

ALLOWED: Writing tests first
ALLOWED: Implementation to make tests pass
ALLOWED: Refactoring while tests stay green
```

## Test Structure

```typescript
describe('FeatureName', () => {
  // Setup
  beforeEach(() => { /* setup */ });

  // Happy path first
  it('should do the primary thing', () => {
    expect(result).toBe(expected);
  });

  // Edge cases
  it('should handle edge case X', () => {});

  // Error cases
  it('should throw when invalid input', () => {});
});
```

## Coverage Requirements

- **Minimum**: 70% coverage
- **Target**: 80%+ coverage
- **Critical paths**: 100% coverage (auth, payments)

## What NOT to Test

- Third-party library internals
- Simple getters/setters with no logic
- Configuration constants
- Framework boilerplate
