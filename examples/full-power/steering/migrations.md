---
inclusion: always
---

# Database Migrations Guide

## Overview

This guide helps you create safe, reversible database migrations. Follow these practices to avoid data loss and downtime.

## Migration Structure

Every migration should have:
1. **Up migration** - applies the change
2. **Down migration** - reverses the change
3. **Description** - explains what and why

## Safe Migration Patterns

### Adding a Column

```sql
-- Up
ALTER TABLE users ADD COLUMN phone VARCHAR(20);

-- Down
ALTER TABLE users DROP COLUMN phone;
```

### Adding a NOT NULL Column

Do it in steps to avoid locking:

```sql
-- Step 1: Add nullable column
ALTER TABLE users ADD COLUMN status VARCHAR(20);

-- Step 2: Backfill data
UPDATE users SET status = 'active' WHERE status IS NULL;

-- Step 3: Add constraint
ALTER TABLE users ALTER COLUMN status SET NOT NULL;
```

### Renaming a Column

```sql
-- Up
ALTER TABLE users RENAME COLUMN name TO full_name;

-- Down
ALTER TABLE users RENAME COLUMN full_name TO name;
```

### Adding an Index

Use CONCURRENTLY to avoid locking:

```sql
-- Up
CREATE INDEX CONCURRENTLY idx_users_email ON users(email);

-- Down
DROP INDEX idx_users_email;
```

## Dangerous Operations

### Dropping a Column

Always backup first:

```sql
-- Create backup
CREATE TABLE users_backup AS SELECT * FROM users;

-- Then drop
ALTER TABLE users DROP COLUMN old_column;
```

### Changing Column Type

May require data migration:

```sql
-- Add new column
ALTER TABLE orders ADD COLUMN amount_new DECIMAL(19,4);

-- Migrate data
UPDATE orders SET amount_new = amount::DECIMAL(19,4);

-- Swap columns
ALTER TABLE orders DROP COLUMN amount;
ALTER TABLE orders RENAME COLUMN amount_new TO amount;
```

## Testing Migrations

1. Run on a copy of production data
2. Verify data integrity after migration
3. Test the rollback (down migration)
4. Measure migration time on realistic data

## Migration Checklist

- [ ] Does the up migration work?
- [ ] Does the down migration work?
- [ ] Is the migration idempotent?
- [ ] Will it lock tables for too long?
- [ ] Is there a data backup plan?
- [ ] Has it been tested on production-like data?
