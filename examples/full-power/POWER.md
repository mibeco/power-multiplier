---
name: "database-assistant"
displayName: "Database Assistant"
description: "Comprehensive database management with schema design, query optimization, and migration assistance"
keywords: ["database", "db", "sql", "schema", "migration", "query", "postgres", "mysql"]
author: "Example Author"
mcpServers: ["postgres-mcp", "schema-analyzer"]
---

# Database Assistant

## Overview

Database Assistant is a comprehensive power for database management tasks. It combines MCP servers for direct database interaction with steering files that provide workflow-specific guidance for schema design, query optimization, and migrations.

## Available MCP Servers

### postgres-mcp

Direct PostgreSQL database interaction.

| Tool | Description |
|------|-------------|
| `query` | Execute a SQL query and return results |
| `describe_table` | Get table schema information |
| `list_tables` | List all tables in the database |

### schema-analyzer

Analyze and optimize database schemas.

| Tool | Description |
|------|-------------|
| `analyze_schema` | Analyze schema for optimization opportunities |
| `suggest_indexes` | Suggest indexes based on query patterns |
| `check_normalization` | Check normalization level of tables |

## When to Load Steering Files

| Workflow | Steering File | When to Use |
|----------|---------------|-------------|
| Designing schemas | `steering/schema-design.md` | Creating new tables or modifying existing ones |
| Writing queries | `steering/query-optimization.md` | Optimizing slow queries or writing complex SQL |
| Running migrations | `steering/migrations.md` | Creating and running database migrations |

## Setup

### Environment Variables

Set these environment variables before using:

```bash
export DATABASE_URL="postgresql://user:pass@localhost:5432/dbname"
```

### Prerequisites

- PostgreSQL 12+ installed and running
- Database connection credentials
- uvx installed for MCP server execution

## Common Workflows

### Design a New Schema

Ask: "Help me design a schema for [use case]"

The schema-design steering file will guide you through:
1. Identifying entities and relationships
2. Choosing appropriate data types
3. Setting up primary and foreign keys
4. Adding indexes for common queries

### Optimize a Slow Query

Ask: "This query is slow: [paste query]"

The query-optimization steering file will:
1. Analyze the query execution plan
2. Identify missing indexes
3. Suggest query rewrites
4. Recommend schema changes if needed

### Create a Migration

Ask: "Create a migration to [describe change]"

The migrations steering file will:
1. Generate migration SQL
2. Create rollback script
3. Validate against existing schema
4. Provide testing guidance

## Best Practices

- Always test migrations on a copy of production data
- Use transactions for multi-statement changes
- Index foreign keys and frequently queried columns
- Normalize to 3NF unless you have specific performance needs
- Document schema decisions in migration files
