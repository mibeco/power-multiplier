---
inclusion: always
---

# Schema Design Guide

## Overview

This guide helps you design effective database schemas. Follow these steps when creating new tables or modifying existing ones.

## Design Process

### 1. Identify Entities

List all the things you need to store:
- What are the main objects in your domain?
- What attributes does each object have?
- What relationships exist between objects?

### 2. Define Relationships

Determine how entities relate:
- **One-to-One**: Use a foreign key in either table
- **One-to-Many**: Put the foreign key in the "many" table
- **Many-to-Many**: Create a junction table

### 3. Choose Data Types

Select appropriate types for each column:

| Data | Recommended Type |
|------|------------------|
| IDs | `UUID` or `BIGSERIAL` |
| Short text | `VARCHAR(n)` |
| Long text | `TEXT` |
| Money | `DECIMAL(19,4)` |
| Timestamps | `TIMESTAMPTZ` |
| Booleans | `BOOLEAN` |
| JSON data | `JSONB` |

### 4. Add Constraints

Ensure data integrity:
- `PRIMARY KEY` on every table
- `FOREIGN KEY` for relationships
- `NOT NULL` where required
- `UNIQUE` for natural keys
- `CHECK` for value validation

## Example Schema

```sql
CREATE TABLE users (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    email VARCHAR(255) NOT NULL UNIQUE,
    name VARCHAR(100) NOT NULL,
    created_at TIMESTAMPTZ NOT NULL DEFAULT NOW()
);

CREATE TABLE posts (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID NOT NULL REFERENCES users(id),
    title VARCHAR(200) NOT NULL,
    content TEXT,
    published_at TIMESTAMPTZ,
    created_at TIMESTAMPTZ NOT NULL DEFAULT NOW()
);

CREATE INDEX idx_posts_user_id ON posts(user_id);
CREATE INDEX idx_posts_published_at ON posts(published_at);
```

## Common Patterns

### Soft Deletes
```sql
deleted_at TIMESTAMPTZ DEFAULT NULL
```

### Audit Columns
```sql
created_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
updated_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
created_by UUID REFERENCES users(id),
updated_by UUID REFERENCES users(id)
```

### Enum-like Values
```sql
CREATE TYPE status AS ENUM ('draft', 'published', 'archived');
```
