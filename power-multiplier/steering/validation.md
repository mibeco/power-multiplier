---
inclusion: always
---

# Power Validation Guide

## Overview

This guide helps you validate your Kiro Power before publishing or local testing. Validation ensures your power meets Kiro's requirements and will work correctly when installed. A thorough validation catches common issues early, saving time and frustration.

## Validation Workflow

Follow this sequence to validate your power:

1. **Validate POWER.md** - Check metadata and content structure
2. **Validate mcp.json** - Verify server configuration (if present)
3. **Validate steering files** - Check frontmatter and references (if present)
4. **Cross-file validation** - Ensure consistency across files

## POWER.md Validation Checklist

### Frontmatter Validation

| Check | Requirement | Example |
|-------|-------------|---------|
| ✅ Frontmatter exists | YAML between `---` markers | `---\nname: "my-power"\n---` |
| ✅ `name` field | kebab-case, no spaces | `"my-awesome-power"` |
| ✅ `displayName` field | Human-readable string | `"My Awesome Power"` |
| ✅ `description` field | One clear sentence | `"Helps developers do X"` |
| ✅ `keywords` field | Array with 3-8 terms | `["keyword1", "keyword2"]` |
| ⚪ `author` field | Optional but recommended | `"Your Name"` |

### Frontmatter Rules

```yaml
# ✅ Valid frontmatter
---
name: "my-power"
displayName: "My Power"
description: "Helps developers accomplish X with Y"
keywords: ["x", "y", "do x", "help with y"]
author: "Developer Name"
---

# ❌ Invalid: name has spaces
---
name: "my power"
---

# ❌ Invalid: missing required field
---
name: "my-power"
displayName: "My Power"
# Missing description and keywords
---

# ❌ Invalid: keywords not an array
---
keywords: "single keyword"
---
```

### Content Validation

| Check | Requirement |
|-------|-------------|
| ✅ Overview section | Explains what the power does |
| ✅ Key capabilities | Lists main features |
| ⚪ MCP server docs | Required if mcp.json exists |
| ⚪ Best practices | Recommended for all powers |
| ⚪ Steering file table | Required if steering/ exists |

### POWER.md Validation Commands

Run these checks manually:

```bash
# Check frontmatter exists and is valid YAML
head -50 POWER.md | grep -A 100 "^---" | head -n -1

# Verify required fields
grep -E "^name:|^displayName:|^description:|^keywords:" POWER.md

# Check for Overview section
grep -i "## Overview" POWER.md
```

## mcp.json Validation Checklist

### JSON Syntax Validation

| Check | Requirement |
|-------|-------------|
| ✅ Valid JSON | No syntax errors |
| ✅ No trailing commas | JSON doesn't allow trailing commas |
| ✅ Proper quotes | All strings use double quotes |
| ✅ mcpServers object | Top-level key exists |

### Server Configuration Validation

#### Stdio Servers

| Check | Requirement | Example |
|-------|-------------|---------|
| ✅ `command` field | Executable name | `"uvx"`, `"npx"`, `"node"` |
| ✅ `args` field | Array of arguments | `["package@latest"]` |
| ⚪ `env` field | Object of env vars | `{"API_KEY": "${env:KEY}"}` |
| ⚪ `timeout` field | Number in milliseconds | `30000` |

```json
{
  "mcpServers": {
    "my-server": {
      "command": "uvx",
      "args": ["package-name@latest"],
      "env": {
        "API_KEY": "${env:MY_API_KEY}"
      },
      "timeout": 30000
    }
  }
}
```

#### HTTP Servers

| Check | Requirement | Example |
|-------|-------------|---------|
| ✅ `type` field | Must be `"http"` | `"http"` |
| ✅ `url` field | Valid HTTP URL | `"https://api.example.com/mcp"` |

```json
{
  "mcpServers": {
    "my-api": {
      "type": "http",
      "url": "https://api.example.com/mcp"
    }
  }
}
```

#### Remote Servers

| Check | Requirement | Example |
|-------|-------------|---------|
| ✅ `type` field | Must be `"remote"` | `"remote"` |
| ✅ `url` field | Valid MCP URL | `"mcp://service.example.com/v1"` |

```json
{
  "mcpServers": {
    "cloud-service": {
      "type": "remote",
      "url": "mcp://service.example.com/v1"
    }
  }
}
```

### mcp.json Validation Commands

```bash
# Validate JSON syntax
cat mcp.json | python3 -m json.tool > /dev/null && echo "Valid JSON" || echo "Invalid JSON"

# Or with jq
jq . mcp.json > /dev/null && echo "Valid JSON" || echo "Invalid JSON"

# Check for mcpServers key
jq 'has("mcpServers")' mcp.json

# List server names
jq '.mcpServers | keys' mcp.json
```

## Steering Files Validation Checklist

### Frontmatter Validation

| Check | Requirement |
|-------|-------------|
| ✅ Frontmatter exists | YAML between `---` markers |
| ✅ `inclusion` field | One of: `always`, `fileMatch`, `manual` |
| ⚪ `fileMatchPattern` | Required if inclusion is `fileMatch` |

### Valid Frontmatter Examples

```yaml
# Always inclusion
---
inclusion: always
---

# FileMatch inclusion
---
inclusion: fileMatch
fileMatchPattern: "**/*.ts"
---

# Manual inclusion
---
inclusion: manual
---
```

### Invalid Frontmatter Examples

```yaml
# ❌ Missing inclusion field
---
title: "My Guide"
---

# ❌ Invalid inclusion value
---
inclusion: auto
---

# ❌ FileMatch without pattern
---
inclusion: fileMatch
---
```

### Steering File Content Validation

| Check | Requirement |
|-------|-------------|
| ✅ Overview section | Explains the workflow |
| ✅ Clear structure | Organized with headings |
| ⚪ Examples | Code samples where relevant |
| ⚪ Common issues | Troubleshooting section |

### Steering Files Validation Commands

```bash
# Check all steering files have frontmatter
for f in steering/*.md; do
  echo "=== $f ==="
  head -10 "$f" | grep -A 5 "^---"
done

# Verify inclusion field exists
for f in steering/*.md; do
  grep -l "^inclusion:" "$f" || echo "Missing inclusion: $f"
done

# List all steering files
ls -la steering/*.md
```

## Cross-File Validation

### Server Name Consistency

If POWER.md references MCP servers in frontmatter, they must exist in mcp.json:

```yaml
# POWER.md frontmatter
---
name: "my-power"
mcpServers:
  - database
  - storage
---
```

```json
// mcp.json must have these servers
{
  "mcpServers": {
    "database": { ... },
    "storage": { ... }
  }
}
```

**Validation:**
```bash
# Extract server names from POWER.md (if mcpServers in frontmatter)
grep -A 10 "mcpServers:" POWER.md | grep "^\s*-" | sed 's/.*- //'

# Extract server names from mcp.json
jq '.mcpServers | keys[]' mcp.json
```

### Steering File References

All steering files should be referenced in POWER.md:

```markdown
## When to Load Steering Files

| Workflow | Steering File | When to Use |
|----------|---------------|-------------|
| Setup | `steering/setup.md` | Initial configuration |
| Development | `steering/development.md` | Writing code |
```

**Validation:**
```bash
# List steering files
ls steering/*.md | xargs -n1 basename

# Check references in POWER.md
grep -o "steering/[a-z-]*.md" POWER.md | sort -u
```

## Common Errors and Remediation

### POWER.md Errors

| Error | Cause | Remediation |
|-------|-------|-------------|
| Missing POWER.md | File not created | Create POWER.md with required frontmatter |
| Invalid YAML frontmatter | Syntax error in YAML | Check for proper `---` delimiters, indentation |
| Missing `name` field | Required field absent | Add `name: "your-power-name"` to frontmatter |
| Missing `displayName` field | Required field absent | Add `displayName: "Your Power Name"` |
| Missing `description` field | Required field absent | Add `description: "What your power does"` |
| Missing `keywords` field | Required field absent | Add `keywords: ["keyword1", "keyword2"]` |
| Invalid `name` format | Contains spaces or uppercase | Use kebab-case: `my-power-name` |
| Empty `keywords` array | No keywords provided | Add 3-8 relevant keywords |

### mcp.json Errors

| Error | Cause | Remediation |
|-------|-------|-------------|
| Invalid JSON | Syntax error | Remove trailing commas, fix quotes |
| Missing `mcpServers` | Top-level key absent | Add `"mcpServers": {}` wrapper |
| Missing `command` | Stdio server incomplete | Add `"command": "uvx"` or similar |
| Missing `args` | Stdio server incomplete | Add `"args": ["package@latest"]` |
| Missing `type` | HTTP/remote server incomplete | Add `"type": "http"` or `"type": "remote"` |
| Missing `url` | HTTP/remote server incomplete | Add `"url": "https://..."` |
| Invalid env syntax | Wrong variable format | Use `"${env:VARIABLE_NAME}"` |

### Steering File Errors

| Error | Cause | Remediation |
|-------|-------|-------------|
| Missing frontmatter | No YAML block | Add `---\ninclusion: always\n---` |
| Missing `inclusion` | Required field absent | Add `inclusion: always\|fileMatch\|manual` |
| Invalid `inclusion` value | Typo or wrong value | Use exactly: `always`, `fileMatch`, or `manual` |
| Missing `fileMatchPattern` | FileMatch without pattern | Add `fileMatchPattern: "**/*.ext"` |
| Invalid glob pattern | Syntax error in pattern | Check glob syntax, use `**` for recursive |
| Orphaned steering file | Not referenced in POWER.md | Add to "When to Load Steering Files" table |

### Cross-File Errors

| Error | Cause | Remediation |
|-------|-------|-------------|
| Server name mismatch | POWER.md references non-existent server | Add server to mcp.json or remove reference |
| Unreferenced steering file | File exists but not in POWER.md | Add to steering file table in POWER.md |
| Duplicate server names | Same name in multiple places | Use unique server names |

## Complete Validation Script

Run this script to validate your entire power:

```bash
#!/bin/bash
# validate-power.sh

echo "=== Power Validation ==="
ERRORS=0

# Check POWER.md exists
if [ ! -f "POWER.md" ]; then
  echo "❌ POWER.md not found"
  ERRORS=$((ERRORS + 1))
else
  echo "✅ POWER.md exists"
  
  # Check frontmatter
  if head -1 POWER.md | grep -q "^---"; then
    echo "✅ Frontmatter delimiter found"
  else
    echo "❌ Missing frontmatter"
    ERRORS=$((ERRORS + 1))
  fi
  
  # Check required fields
  for field in "name:" "displayName:" "description:" "keywords:"; do
    if grep -q "^$field" POWER.md; then
      echo "✅ $field field found"
    else
      echo "❌ Missing $field field"
      ERRORS=$((ERRORS + 1))
    fi
  done
fi

# Check mcp.json if exists
if [ -f "mcp.json" ]; then
  echo "--- mcp.json ---"
  if cat mcp.json | python3 -m json.tool > /dev/null 2>&1; then
    echo "✅ Valid JSON syntax"
  else
    echo "❌ Invalid JSON syntax"
    ERRORS=$((ERRORS + 1))
  fi
fi

# Check steering files if directory exists
if [ -d "steering" ]; then
  echo "--- Steering Files ---"
  for f in steering/*.md; do
    if [ -f "$f" ]; then
      if head -1 "$f" | grep -q "^---"; then
        if grep -q "^inclusion:" "$f"; then
          echo "✅ $f has valid frontmatter"
        else
          echo "❌ $f missing inclusion field"
          ERRORS=$((ERRORS + 1))
        fi
      else
        echo "❌ $f missing frontmatter"
        ERRORS=$((ERRORS + 1))
      fi
    fi
  done
fi

echo "=== Validation Complete ==="
if [ $ERRORS -eq 0 ]; then
  echo "✅ All checks passed!"
else
  echo "❌ Found $ERRORS error(s)"
fi
```

## Validation Summary

After validation, your power should have:

### Required
- [ ] POWER.md with valid YAML frontmatter
- [ ] `name` field (kebab-case)
- [ ] `displayName` field (human-readable)
- [ ] `description` field (one sentence)
- [ ] `keywords` field (array with 3-8 terms)
- [ ] Overview section in POWER.md

### If Using MCP Servers
- [ ] mcp.json with valid JSON syntax
- [ ] Each server has required fields for its type
- [ ] Server names match any POWER.md references
- [ ] Environment variables use correct syntax

### If Using Steering Files
- [ ] Each file has YAML frontmatter
- [ ] Each file has `inclusion` field
- [ ] FileMatch files have `fileMatchPattern`
- [ ] All files referenced in POWER.md

## Next Steps

After validation passes:

1. **Test locally** → See `steering/testing.md`
2. **Publish to GitHub** → See `steering/publishing.md`

