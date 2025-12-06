---
inclusion: always
---

# Steering Files Guide

## Overview

Steering files provide workflow-specific guidance that loads dynamically based on context. They allow you to organize detailed instructions, templates, and best practices into separate files that the AI agent loads when relevant, keeping your POWER.md focused and your guidance contextual.

## When to Use Steering Files

### Use Steering Files When:

- Your power supports multiple distinct workflows
- Different file types need different guidance
- Deep-dive documentation would clutter POWER.md
- You want context-sensitive loading based on file patterns
- Users need step-by-step guides for complex tasks

### Keep Content in POWER.md When:

- Guidance is brief and applies universally
- You have a simple power with one workflow
- Content is essential for understanding the power
- Information is needed regardless of context

## Inclusion Types

Steering files support three inclusion modes that control when they load:

### 1. Always Inclusion

```yaml
---
inclusion: always
---
```

**Behavior:** File loads whenever the power is activated.

**Use for:**
- Core workflows that apply to all users
- Essential guidance that's always relevant
- Getting started documentation
- Best practices that apply universally

**Example use cases:**
- Scaffolding guides
- Configuration documentation
- Style guides
- Security best practices

### 2. FileMatch Inclusion

```yaml
---
inclusion: fileMatch
fileMatchPattern: "**/*.ts"
---
```

**Behavior:** File loads only when the user has a matching file open.

**Use for:**
- Language-specific guidance
- File-type-specific workflows
- Context-aware documentation
- Technology-specific best practices

**Example use cases:**
- TypeScript-specific patterns when editing `.ts` files
- React guidance when editing `.tsx` or `.jsx` files
- Test writing guidance when editing `*.test.ts` files
- Migration guidance when editing `migrations/*.sql` files

### 3. Manual Inclusion

```yaml
---
inclusion: manual
---
```

**Behavior:** File only loads when explicitly requested via `#` context key.

**Use for:**
- Advanced topics not needed by most users
- Reference documentation
- Troubleshooting guides
- Deep-dive technical content

**Example use cases:**
- Advanced configuration options
- Performance tuning guides
- Debugging documentation
- Migration guides from other tools

## FileMatchPattern Glob Syntax

The `fileMatchPattern` field uses glob patterns to match files:

### Basic Patterns

| Pattern | Matches |
|---------|---------|
| `*.ts` | TypeScript files in current directory |
| `**/*.ts` | TypeScript files in any directory |
| `src/**/*.ts` | TypeScript files under src/ |
| `*.{ts,tsx}` | TypeScript and TSX files |
| `test/**/*` | All files under test/ |

### Common Patterns

```yaml
# JavaScript/TypeScript
fileMatchPattern: "**/*.{js,jsx,ts,tsx}"

# React components
fileMatchPattern: "**/*.{jsx,tsx}"

# Test files
fileMatchPattern: "**/*.{test,spec}.{js,ts}"

# Configuration files
fileMatchPattern: "**/*.{json,yaml,yml}"

# Markdown documentation
fileMatchPattern: "**/*.md"

# SQL files
fileMatchPattern: "**/*.sql"

# Python files
fileMatchPattern: "**/*.py"

# Specific directories
fileMatchPattern: "src/components/**/*"

# Multiple extensions
fileMatchPattern: "**/*.{css,scss,less}"
```

### Pattern Rules

- `*` matches any characters except `/`
- `**` matches any characters including `/`
- `?` matches a single character
- `{a,b}` matches either `a` or `b`
- `[abc]` matches any character in the set
- `[!abc]` matches any character not in the set

## Steering File Structure

### Basic Template

```markdown
---
inclusion: always
---

# [Workflow Name]

## Overview

[Brief description of what this workflow accomplishes]

## When to Use

[Situations where this workflow applies]

## Steps

### Step 1: [Name]

[Description]

```code
[Example]
```

### Step 2: [Name]

[Description]

```code
[Example]
```

## Templates

[Reusable code/content templates]

## Common Issues

### Issue: [Problem]
**Cause:** [Why it happens]
**Solution:** [How to fix it]

## Related

- [Link to related steering file]
- [Link to external resource]
```

### FileMatch Template

```markdown
---
inclusion: fileMatch
fileMatchPattern: "**/*.ts"
---

# TypeScript Development Guide

## Overview

This guide provides TypeScript-specific patterns and best practices.

## File Context

This guidance applies when working with TypeScript files (`.ts`).

## Patterns

### Pattern 1: [Name]

[Description and example]

### Pattern 2: [Name]

[Description and example]

## Type Definitions

[TypeScript-specific type guidance]

## Common Mistakes

[TypeScript-specific anti-patterns to avoid]
```

### Manual Template

```markdown
---
inclusion: manual
---

# Advanced Configuration Guide

## Overview

This guide covers advanced configuration options for power users.

## How to Access

Reference this guide using `#advanced-config` in your conversation.

## Advanced Options

### Option 1: [Name]

[Detailed explanation]

### Option 2: [Name]

[Detailed explanation]

## Performance Tuning

[Advanced performance guidance]

## Debugging

[Advanced debugging techniques]
```

## Organizing Steering Files

### By Workflow

```
steering/
├── getting-started.md      # inclusion: always
├── configuration.md        # inclusion: always
├── workflow-a.md           # inclusion: always
├── workflow-b.md           # inclusion: always
└── troubleshooting.md      # inclusion: manual
```

### By File Type

```
steering/
├── core.md                 # inclusion: always
├── typescript.md           # inclusion: fileMatch, **/*.ts
├── react.md                # inclusion: fileMatch, **/*.tsx
├── testing.md              # inclusion: fileMatch, **/*.test.ts
└── advanced.md             # inclusion: manual
```

### By User Journey

```
steering/
├── 01-setup.md             # inclusion: always
├── 02-basics.md            # inclusion: always
├── 03-intermediate.md      # inclusion: manual
├── 04-advanced.md          # inclusion: manual
└── 05-troubleshooting.md   # inclusion: manual
```

## Referencing Steering Files in POWER.md

Always document your steering files in POWER.md so users know what's available:

### Table Format (Recommended)

```markdown
## When to Load Steering Files

| Workflow | Steering File | When to Use |
|----------|---------------|-------------|
| Getting started | `steering/setup.md` | Initial power setup |
| TypeScript development | `steering/typescript.md` | Working with .ts files |
| Testing | `steering/testing.md` | Writing tests |
| Advanced config | `steering/advanced.md` | Manual: `#advanced` |
```

### List Format

```markdown
## Available Steering Files

- **setup.md** - Getting started with the power (always loaded)
- **typescript.md** - TypeScript patterns (loads with .ts files)
- **testing.md** - Test writing guidance (loads with test files)
- **advanced.md** - Advanced configuration (manual: use `#advanced`)
```

## Best Practices

### ✅ Do:

- Keep each steering file focused on one workflow or topic
- Use descriptive file names that indicate content
- Include an overview section explaining when to use the guide
- Provide concrete examples and templates
- Reference related steering files
- Document all steering files in POWER.md

### ❌ Don't:

- Create steering files for content that fits in POWER.md
- Use vague file names like `guide.md` or `help.md`
- Duplicate content across multiple steering files
- Forget to update POWER.md when adding steering files
- Create too many small steering files (consolidate related content)
- Use fileMatch patterns that are too broad

## Inline vs Separate Steering

### Use Inline Guidance (in POWER.md) When:

| Scenario | Recommendation |
|----------|----------------|
| Content is < 100 lines | Keep inline |
| Applies to all users | Keep inline |
| Essential for understanding | Keep inline |
| Single workflow power | Keep inline |

### Use Separate Steering Files When:

| Scenario | Recommendation |
|----------|----------------|
| Content is > 100 lines | Separate file |
| Multiple distinct workflows | Separate files |
| File-type-specific guidance | FileMatch steering |
| Advanced/optional content | Manual steering |
| Deep-dive documentation | Separate file |

## Examples

### Example 1: API Development Power

```
steering/
├── design.md           # inclusion: always - API design patterns
├── openapi.md          # inclusion: fileMatch, **/*.yaml - OpenAPI specs
├── implementation.md   # inclusion: fileMatch, **/*.ts - Endpoint code
├── testing.md          # inclusion: fileMatch, **/*.test.ts - API tests
└── deployment.md       # inclusion: manual - Deployment guide
```

### Example 2: Documentation Power

```
steering/
├── writing.md          # inclusion: always - Writing guidelines
├── markdown.md         # inclusion: fileMatch, **/*.md - Markdown files
├── asciidoc.md         # inclusion: fileMatch, **/*.adoc - AsciiDoc files
└── publishing.md       # inclusion: manual - Publishing workflow
```

### Example 3: Database Power

```
steering/
├── queries.md          # inclusion: always - Query patterns
├── migrations.md       # inclusion: fileMatch, **/migrations/*.sql
├── schema.md           # inclusion: fileMatch, **/*.prisma
└── performance.md      # inclusion: manual - Performance tuning
```

## Validation Checklist

Before finalizing your steering files:

- [ ] Each file has valid YAML frontmatter with `inclusion` field
- [ ] FileMatch files have valid `fileMatchPattern` glob patterns
- [ ] File names are descriptive and use kebab-case
- [ ] Each file has an Overview section
- [ ] Content is focused on a single workflow/topic
- [ ] All steering files are referenced in POWER.md
- [ ] No duplicate content across files
- [ ] Templates and examples are included where helpful

## Common Issues

### Issue: Steering file not loading

**Cause:** Invalid frontmatter or missing inclusion field
**Solution:** Verify YAML syntax and ensure `inclusion` field is present

### Issue: FileMatch not triggering

**Cause:** Glob pattern doesn't match open file
**Solution:** Test pattern with actual file paths, use `**` for recursive matching

### Issue: Too many files loading

**Cause:** Overly broad fileMatch patterns
**Solution:** Make patterns more specific, consider using manual inclusion

### Issue: Content not found

**Cause:** Steering file not referenced in POWER.md
**Solution:** Add reference to "When to Load Steering Files" section

## Next Steps

After creating steering files:

1. **Update POWER.md** with steering file references
2. **Validate your power** → See `steering/validation.md`
3. **Test locally** → See `steering/testing.md`
4. **Publish to GitHub** → See `steering/publishing.md`
