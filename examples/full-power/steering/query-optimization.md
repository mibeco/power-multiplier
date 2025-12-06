---
inclusion: always
---

# Query Optimization Guide

## Overview

This guide helps you identify and fix slow queries. Use it when queries are taking too long or consuming too many resources.

## Diagnosis Steps

### 1. Get the Execution Plan

```sql
EXPLAIN ANALYZE SELECT ...;
```

Look for:
- **Seq Scan** on large tables (usually bad)
- **High cost** estimates
- **Loops** with high iteration counts
- **Sort** operations on large datasets

### 2. Check for Missing Indexes

Common candidates for indexes:
- Foreign key columns
- Columns in WHERE clauses
- Columns in ORDER BY
- Columns in JOIN conditions

### 3. Analyze Table Statistics

```sql
ANALYZE table_name;
```

Ensures the query planner has accurate statistics.

## Common Optimizations

### Add Indexes

```sql
-- Single column index
CREATE INDEX idx_users_email ON users(email);

-- Composite index (order matters!)
CREATE INDEX idx_orders_user_date ON orders(user_id, created_at);

-- Partial index
CREATE INDEX idx_active_users ON users(email) WHERE active = true;
```

### Rewrite Queries

**Before (slow):**
```sql
SELECT * FROM orders WHERE YEAR(created_at) = 2024;
```

**After (fast):**
```sql
SELECT * FROM orders 
WHERE created_at >= '2024-01-01' 
  AND created_at < '2025-01-01';
```

### Use LIMIT for Pagination

```sql
SELECT * FROM posts 
ORDER BY created_at DESC 
LIMIT 20 OFFSET 0;
```

### Avoid SELECT *

```sql
-- Bad
SELECT * FROM users;

-- Good
SELECT id, name, email FROM users;
```

## Index Types

| Type | Use Case |
|------|----------|
| B-tree | Default, good for most cases |
| Hash | Equality comparisons only |
| GIN | Full-text search, JSONB |
| GiST | Geometric data, ranges |

## When NOT to Index

- Small tables (< 1000 rows)
- Columns with low cardinality
- Tables with heavy write load
- Columns rarely used in queries
