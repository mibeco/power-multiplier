---
inclusion: always
---

# POWER.md Writing Guide

## Overview

This guide helps you write effective POWER.md content that provides clear instructions to the AI agent. A well-written POWER.md is the foundation of a successful Kiro Power—it determines how the agent understands and uses your power's capabilities.

## Required Sections

Every POWER.md should include these sections:

### 1. Frontmatter (Required)

The YAML frontmatter at the top defines your power's metadata:

```yaml
---
name: "your-power-name"           # kebab-case identifier
displayName: "Your Power Name"    # Human-readable name
description: "One-line description of what your power does"
keywords: ["keyword1", "keyword2", "action phrase"]
author: "Your Name"               # Optional but recommended
---
```

### 2. Overview Section (Required)

Start with a clear explanation of what your power does:

```markdown
# Your Power Name

## Overview

[2-3 sentences explaining what the power does and who it's for]

**Key capabilities:**
- [Capability 1]
- [Capability 2]
- [Capability 3]

**Perfect for:**
- [Use case 1]
- [Use case 2]
- [Use case 3]
```

### 3. Available MCP Servers (If Applicable)

Document each MCP server your power provides:

```markdown
## Available MCP Servers

### server-name

**Package:** `package-name@latest`
**Connection:** [stdio | http | remote]

**Tools:**

1. **tool_name** - Brief description of what the tool does
   - Required: `param1` (type) - Description
   - Optional: `param2` (type) - Description
   - Returns: Description of return value
```

### 4. Best Practices Section (Recommended)

Provide guidance on effective usage:

```markdown
## Best Practices

### ✅ Do:
- [Best practice 1]
- [Best practice 2]
- [Best practice 3]

### ❌ Don't:
- [Anti-pattern 1]
- [Anti-pattern 2]
- [Anti-pattern 3]
```

### 5. Common Workflows Section (Recommended)

Show how to accomplish common tasks:

```markdown
## Common Workflows

### Workflow 1: [Task Name]

[Brief description of what this workflow accomplishes]

```javascript
// Step 1: [Description]
[code example]

// Step 2: [Description]
[code example]
```
```

## Templates by Power Type

### Simple Power (Documentation Only)

For powers that provide context and guidance without MCP servers:

```markdown
---
name: "style-guide"
displayName: "Style Guide"
description: "Enforce consistent code style and documentation standards"
keywords: ["style", "formatting", "conventions", "standards"]
author: "Your Name"
---

# Style Guide Power

## Overview

This power provides guidance on code style and documentation standards for [language/framework]. It helps maintain consistency across your codebase by providing context-aware suggestions.

**Key capabilities:**
- Naming convention guidance
- Documentation formatting standards
- Code organization patterns

## Style Guidelines

### Naming Conventions

- **Variables**: Use camelCase for variables and functions
- **Classes**: Use PascalCase for class names
- **Constants**: Use UPPER_SNAKE_CASE for constants

### Documentation Standards

- Every public function should have a docstring
- Include parameter types and return types
- Provide usage examples for complex functions

## Best Practices

### ✅ Do:
- Follow consistent naming patterns
- Document public APIs thoroughly
- Use meaningful variable names

### ❌ Don't:
- Mix naming conventions
- Leave public functions undocumented
- Use single-letter variable names (except in loops)
```

### MCP Power (With Tools)

For powers that wrap MCP servers:

```markdown
---
name: "database-helper"
displayName: "Database Helper"
description: "Manage database connections, queries, and migrations"
keywords: ["database", "sql", "postgres", "mysql", "migrations"]
author: "Your Name"
---

# Database Helper Power

## Overview

Database Helper provides tools for managing database connections, executing queries, and handling migrations. It supports PostgreSQL and MySQL databases.

**Key capabilities:**
- Execute SQL queries safely
- Manage database connections
- Run and track migrations
- Generate schema documentation

**Authentication**: Requires database connection string in environment variable.

## Available MCP Servers

### database

**Package:** `database-mcp-server@latest`
**Connection:** stdio via uvx

**Tools:**

1. **execute_query** - Execute a SQL query
   - Required: `query` (string) - SQL query to execute
   - Optional: `database` (string) - Database name (defaults to primary)
   - Returns: Query results with rows and metadata

2. **list_tables** - List all tables in database
   - Optional: `schema` (string) - Schema name (defaults to public)
   - Returns: Array of table names with column info

3. **run_migration** - Execute a migration file
   - Required: `file` (string) - Path to migration file
   - Returns: Migration status and applied changes

## Tool Usage Examples

### Executing Queries

```javascript
// Simple query
usePower("database-helper", "database", "execute_query", {
  "query": "SELECT * FROM users WHERE active = true"
})

// Query with specific database
usePower("database-helper", "database", "execute_query", {
  "query": "SELECT COUNT(*) FROM orders",
  "database": "analytics"
})
```

### Listing Tables

```javascript
usePower("database-helper", "database", "list_tables", {
  "schema": "public"
})
// Returns: [{ name: "users", columns: [...] }, ...]
```

## Common Workflows

### Workflow 1: Database Setup

```javascript
// Step 1: List existing tables
const tables = usePower("database-helper", "database", "list_tables", {})

// Step 2: Create new table if needed
usePower("database-helper", "database", "execute_query", {
  "query": `
    CREATE TABLE IF NOT EXISTS users (
      id SERIAL PRIMARY KEY,
      email VARCHAR(255) UNIQUE NOT NULL,
      created_at TIMESTAMP DEFAULT NOW()
    );
  `
})
```

## Configuration

**Environment Variables:**
- `DATABASE_URL`: Connection string (required)
- `DATABASE_SSL`: Enable SSL (optional, default: true)

**MCP Configuration:**

```json
{
  "mcpServers": {
    "database": {
      "command": "uvx",
      "args": ["database-mcp-server@latest"],
      "env": {
        "DATABASE_URL": "${env:DATABASE_URL}"
      }
    }
  }
}
```

## Best Practices

### ✅ Do:
- Use parameterized queries to prevent SQL injection
- Test queries on development database first
- Back up data before running migrations
- Use transactions for multi-step operations

### ❌ Don't:
- Execute raw user input as SQL
- Run migrations without backups
- Use production credentials in development
- Ignore query performance

## Troubleshooting

### Error: "Connection refused"
**Cause:** Database server not running or incorrect connection string
**Solution:** Verify DATABASE_URL and ensure database is accessible

### Error: "Permission denied"
**Cause:** Database user lacks required permissions
**Solution:** Grant appropriate permissions to the database user
```

### Full Power (With Steering Files)

For complex powers with multiple workflows:

```markdown
---
name: "api-builder"
displayName: "API Builder"
description: "Design, implement, and document REST APIs with best practices"
keywords: ["api", "rest", "openapi", "swagger", "endpoints"]
author: "Your Name"
---

# API Builder Power

## Overview

API Builder helps you design, implement, and document REST APIs following industry best practices. It provides tools for generating OpenAPI specs, scaffolding endpoints, and creating documentation.

**Key capabilities:**
- Generate OpenAPI/Swagger specifications
- Scaffold API endpoints from specs
- Create API documentation
- Validate request/response schemas

**Perfect for:**
- Building new REST APIs
- Documenting existing APIs
- Migrating to OpenAPI standards

## When to Load Steering Files

| Workflow | Steering File | When to Use |
|----------|---------------|-------------|
| Designing API | `steering/design.md` | Planning endpoints and schemas |
| Implementing endpoints | `steering/implementation.md` | Writing endpoint handlers |
| Writing documentation | `steering/documentation.md` | Creating API docs |
| Testing APIs | `steering/testing.md` | Writing API tests |

## Available MCP Servers

### openapi

**Package:** `openapi-mcp-server@latest`
**Connection:** stdio via npx

**Tools:**

1. **generate_spec** - Generate OpenAPI specification
   - Required: `endpoints` (array) - Endpoint definitions
   - Optional: `version` (string) - OpenAPI version (default: 3.0)
   - Returns: OpenAPI specification JSON

2. **validate_spec** - Validate an OpenAPI specification
   - Required: `spec` (object) - OpenAPI specification
   - Returns: Validation results with errors/warnings

## Quick Start

### Design an API

Ask: "Help me design a REST API for [your use case]"

The design workflow will:
1. Gather requirements for your API
2. Suggest endpoint structure
3. Define request/response schemas
4. Generate OpenAPI specification

### Implement Endpoints

Ask: "Implement the endpoints from my OpenAPI spec"

The implementation workflow will:
1. Read your OpenAPI specification
2. Generate endpoint handlers
3. Add validation middleware
4. Create error handling

## Best Practices

### API Design
- Use nouns for resources, verbs for actions
- Version your API (e.g., /v1/users)
- Use consistent naming conventions
- Return appropriate HTTP status codes

### Security
- Always validate input
- Use authentication for protected endpoints
- Implement rate limiting
- Log security-relevant events

### Documentation
- Document all endpoints
- Include request/response examples
- Explain error codes
- Keep docs in sync with code

## Configuration

**MCP Configuration:**

```json
{
  "mcpServers": {
    "openapi": {
      "command": "npx",
      "args": ["-y", "openapi-mcp-server@latest"]
    }
  }
}
```
```

## Writing Effective Content

### Onboarding Instructions

Help users get started quickly:

```markdown
## Getting Started

### Prerequisites
- [Requirement 1]
- [Requirement 2]

### Quick Start

1. **Install the power** via Kiro Powers panel
2. **Configure** [any required settings]
3. **Try it out**: Ask "[example prompt]"

### First Steps

Ask Kiro: "[example prompt that demonstrates core functionality]"

The power will:
1. [What happens first]
2. [What happens next]
3. [Final result]
```

### MCP Server Documentation

Document each tool thoroughly:

```markdown
### tool_name

**Purpose:** [What this tool does]

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| param1 | string | Yes | Description |
| param2 | number | No | Description (default: value) |

**Returns:** [Description of return value]

**Example:**
```javascript
usePower("power-name", "server-name", "tool_name", {
  "param1": "value",
  "param2": 42
})
// Returns: { result: "..." }
```
```

### Workflow Documentation

Show complete workflows with context:

```markdown
### Workflow: [Name]

**Goal:** [What this workflow accomplishes]

**When to use:** [Situations where this workflow applies]

**Steps:**

1. **[Step name]** - [Brief description]
   ```javascript
   // Code example
   ```

2. **[Step name]** - [Brief description]
   ```javascript
   // Code example
   ```

**Result:** [What the user ends up with]
```

## Examples from Official Powers

### Stripe Power Pattern

The Stripe power demonstrates excellent documentation for a payment integration:

- Clear overview of capabilities (Checkout, Payment Intents, Subscriptions)
- Authentication requirements prominently displayed
- Best practices with ✅ Do and ❌ Don't lists
- Complete workflow examples with code
- Troubleshooting section for common errors

**Key takeaway:** Document the "happy path" and common error scenarios.

### Neon Power Pattern

The Neon power shows effective MCP server documentation:

- Each tool documented with parameters and return values
- Tool usage examples with realistic data
- Multi-step workflows showing tool combinations
- Environment-specific guidance (dev, staging, prod)

**Key takeaway:** Show how tools work together, not just individually.

### Cloud Architect Power Pattern

The Cloud Architect power demonstrates steering file integration:

- Overview of included MCP servers
- Reference to steering files for detailed guidance
- Best practices specific to the domain (AWS CDK)
- Example workflow with code

**Key takeaway:** Use steering files for deep-dive content, keep POWER.md focused.

## Common Mistakes to Avoid

### ❌ Too Vague

```markdown
## Overview
This power helps with databases.
```

### ✅ Specific and Actionable

```markdown
## Overview
Database Helper provides tools for executing SQL queries, managing connections, and running migrations. It supports PostgreSQL and MySQL with automatic connection pooling.

**Key capabilities:**
- Execute parameterized SQL queries safely
- List tables and inspect schemas
- Run and rollback migrations
- Generate schema documentation
```

### ❌ Missing Context

```markdown
### execute_query
Runs a query.
```

### ✅ Complete Documentation

```markdown
### execute_query

**Purpose:** Execute a SQL query against the connected database

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| query | string | Yes | SQL query to execute |
| params | array | No | Query parameters for prepared statements |
| database | string | No | Target database (default: primary) |

**Returns:** Object with `rows` (array of results) and `rowCount` (number of affected rows)

**Example:**
```javascript
usePower("db", "database", "execute_query", {
  "query": "SELECT * FROM users WHERE id = $1",
  "params": [123]
})
// Returns: { rows: [{ id: 123, name: "Alice" }], rowCount: 1 }
```
```

## Validation Checklist

Before finalizing your POWER.md:

- [ ] Frontmatter has all required fields (name, displayName, description, keywords)
- [ ] Overview clearly explains what the power does
- [ ] All MCP servers are documented with their tools
- [ ] Each tool has parameters, return values, and examples
- [ ] Best practices section provides actionable guidance
- [ ] At least one workflow example shows end-to-end usage
- [ ] Configuration section explains setup requirements
- [ ] Troubleshooting covers common issues (if applicable)
- [ ] Steering files are referenced in a "When to Load" table (if applicable)

## Next Steps

After writing your POWER.md:

1. **Configure MCP servers** → See `steering/mcp-config.md`
2. **Create steering files** → See `steering/steering-files.md`
3. **Validate your power** → See `steering/validation.md`
4. **Test locally** → See `steering/testing.md`
5. **Publish to GitHub** → See `steering/publishing.md`
