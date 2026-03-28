# Architecture Rules

Think before you code. Every change should make the codebase simpler, not more complex.

## The Three Questions

**BEFORE writing any code, ask:**

1. **Does this fit the existing architecture?**
   - What patterns does this codebase already use?
   - Would this approach surprise another developer?
   - Does it follow the established conventions?

2. **Should I refactor existing code instead?**
   - Can an existing component be extended?
   - Would modifying existing code be simpler than adding new?
   - Is there duplicated logic that should be consolidated?

3. **Is this the simplest solution?**
   - Can this be done with fewer abstractions?
   - Am I adding complexity that isn't needed yet?
   - Would a future developer understand this immediately?

## Codebase Conventions (MANDATORY)

**Always discover and follow existing patterns:**

```
1. BEFORE creating a new file:
   -> Find similar files in the codebase
   -> Match their structure, naming, exports

2. BEFORE adding a new pattern:
   -> Search for how similar problems were solved
   -> Use the existing pattern unless it's fundamentally broken

3. BEFORE adding a dependency:
   -> Check if existing dependencies solve the problem
   -> Check if the codebase has a preferred alternative
```

## Simplicity Principles

### Prefer Modification Over Addition

```
BAD: Adding new file when existing one could be extended
BAD: Creating new abstraction for a single use case
BAD: Adding wrapper when direct usage is clearer

GOOD: Extending existing component with new prop
GOOD: Adding method to existing utility
GOOD: Using framework primitives directly
```

### Prefer Deletion Over Modification

```
BAD: Commenting out code "just in case"
BAD: Adding flags to disable old behavior
BAD: Keeping backwards compatibility for internal code

GOOD: Delete unused code completely
GOOD: Remove old approach when new one works
GOOD: Simplify interfaces by removing rarely-used options
```

### Prefer Explicit Over Clever

```
BAD: Metaprogramming to save a few lines
BAD: Clever type gymnastics that obscure intent
BAD: Abstractions that require reading 5 files to understand

GOOD: Repetition that's easy to understand
GOOD: Straightforward types, even if verbose
GOOD: Code that reads like documentation
```

## Architectural Coherence

### Before Adding New Code

Ask: "If I showed this to the original author, would they say it fits?"

**Check for:**
- Consistent file organization
- Matching naming conventions
- Similar abstraction levels
- Compatible patterns

### When Patterns Conflict

If you find conflicting patterns in the codebase:

1. **Identify the newer/better pattern** -- usually more recent code
2. **Propose consolidation** -- do not add a third pattern
3. **Refactor incrementally** -- update existing code to match

## Interface Design

### Minimal Interfaces

```typescript
// BAD: Kitchen sink interface
interface ButtonProps {
  label: string;
  onClick: () => void;
  variant: 'primary' | 'secondary' | 'tertiary' | 'ghost' | 'link';
  size: 'xs' | 'sm' | 'md' | 'lg' | 'xl';
  disabled?: boolean;
  loading?: boolean;
  icon?: ReactNode;
  iconPosition?: 'left' | 'right';
  fullWidth?: boolean;
  tooltip?: string;
}

// GOOD: Focused interface, compose for variants
interface ButtonProps {
  children: ReactNode;
  onClick: () => void;
  variant?: 'primary' | 'secondary';
  disabled?: boolean;
}
```

### Composition Over Configuration

```typescript
// BAD: One component with many modes
<DataDisplay mode="table" columns={...} rows={...} />
<DataDisplay mode="list" items={...} />
<DataDisplay mode="grid" cards={...} />

// GOOD: Separate components, shared primitives
<DataTable columns={...} rows={...} />
<DataList items={...} />
<DataGrid cards={...} />
```

## The Refactor Test

Before finalizing any implementation, ask:

> "If I came back to this in 6 months, would I understand it immediately,
> or would I wonder why it's so complicated?"

If the latter, simplify.
