---
inclusion: always
---

# MCP Server Configuration Guide

## Overview

This guide helps you configure MCP (Model Context Protocol) servers for your Kiro Power. MCP servers provide tools that the AI agent can use to interact with external services, APIs, and systems.

## Server Types

Kiro supports three types of MCP servers:

| Type | Connection | Best For |
|------|------------|----------|
| **stdio** | Local process via stdin/stdout | CLI tools, local packages (uvx, npx) |
| **http** | HTTP endpoint | REST APIs, web services |
| **remote** | MCP remote URL | Cloud-hosted MCP servers |

## mcp.json Structure

The `mcp.json` file defines all MCP servers for your power:

```json
{
  "mcpServers": {
    "server-name": {
      // Server configuration here
    }
  }
}
```

**Rules:**
- File must be valid JSON
- Server names should be kebab-case
- Each server needs type-specific configuration
- Server names must match any references in POWER.md frontmatter

## Stdio Servers

Stdio servers run as local processes, communicating via stdin/stdout. This is the most common type for CLI tools and packages.

### Configuration Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `command` | string | Yes | Executable to run (e.g., "uvx", "npx", "node") |
| `args` | string[] | Yes | Command arguments |
| `env` | object | No | Environment variables |
| `timeout` | number | No | Timeout in milliseconds (default: 60000) |
| `disabled` | boolean | No | Disable server (default: false) |
| `autoApprove` | string[] | No | Tools to auto-approve |
| `disabledTools` | string[] | No | Tools to disable |

### Template: uvx Package

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

### Template: npx Package

```json
{
  "mcpServers": {
    "my-server": {
      "command": "npx",
      "args": ["-y", "package-name@latest"],
      "env": {
        "NODE_ENV": "production"
      },
      "timeout": 30000
    }
  }
}
```

### Template: Local Script

```json
{
  "mcpServers": {
    "my-server": {
      "command": "node",
      "args": ["./path/to/server.js"],
      "env": {
        "DEBUG": "true"
      }
    }
  }
}
```

### Template: Python Script

```json
{
  "mcpServers": {
    "my-server": {
      "command": "python",
      "args": ["-m", "my_mcp_server"],
      "env": {
        "PYTHONPATH": "${workspaceFolder}"
      }
    }
  }
}
```

### Real-World Examples

**AWS Documentation Server:**
```json
{
  "mcpServers": {
    "aws-docs": {
      "command": "uvx",
      "args": ["awslabs.aws-documentation-mcp-server@latest"],
      "env": {
        "FASTMCP_LOG_LEVEL": "ERROR"
      },
      "timeout": 60000
    }
  }
}
```

**Stripe Server:**
```json
{
  "mcpServers": {
    "stripe": {
      "command": "npx",
      "args": ["-y", "@stripe/mcp@latest"],
      "env": {
        "STRIPE_SECRET_KEY": "${env:STRIPE_SECRET_KEY}"
      },
      "timeout": 30000
    }
  }
}
```

## HTTP Servers

HTTP servers connect to REST API endpoints. Use these for web services that expose an MCP-compatible HTTP interface.

### Configuration Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `type` | string | Yes | Must be "http" |
| `url` | string | Yes | HTTP endpoint URL |
| `timeout` | number | No | Timeout in milliseconds (default: 60000) |
| `disabled` | boolean | No | Disable server (default: false) |
| `autoApprove` | string[] | No | Tools to auto-approve |
| `disabledTools` | string[] | No | Tools to disable |

### Template: HTTP Server

```json
{
  "mcpServers": {
    "my-api": {
      "type": "http",
      "url": "https://api.example.com/mcp",
      "timeout": 30000
    }
  }
}
```

### Template: Local HTTP Server

```json
{
  "mcpServers": {
    "local-api": {
      "type": "http",
      "url": "http://localhost:3000/mcp",
      "timeout": 10000
    }
  }
}
```

## Remote Servers

Remote servers connect to cloud-hosted MCP servers via the MCP remote protocol. Use these for managed MCP services.

### Configuration Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `type` | string | Yes | Must be "remote" |
| `url` | string | Yes | MCP remote URL |
| `timeout` | number | No | Timeout in milliseconds (default: 60000) |
| `disabled` | boolean | No | Disable server (default: false) |
| `autoApprove` | string[] | No | Tools to auto-approve |
| `disabledTools` | string[] | No | Tools to disable |

### Template: Remote Server

```json
{
  "mcpServers": {
    "cloud-service": {
      "type": "remote",
      "url": "mcp://service.example.com/v1",
      "timeout": 45000
    }
  }
}
```

## Environment Variables

Environment variables let you inject secrets and configuration without hardcoding values.

### Syntax

Use `${env:VARIABLE_NAME}` to reference environment variables:

```json
{
  "env": {
    "API_KEY": "${env:MY_API_KEY}",
    "SECRET": "${env:MY_SECRET}",
    "REGION": "${env:AWS_REGION}"
  }
}
```

### Common Patterns

**API Keys:**
```json
{
  "env": {
    "API_KEY": "${env:SERVICE_API_KEY}",
    "API_SECRET": "${env:SERVICE_API_SECRET}"
  }
}
```

**Database Connections:**
```json
{
  "env": {
    "DATABASE_URL": "${env:DATABASE_URL}",
    "DB_SSL": "true"
  }
}
```

**AWS Credentials:**
```json
{
  "env": {
    "AWS_ACCESS_KEY_ID": "${env:AWS_ACCESS_KEY_ID}",
    "AWS_SECRET_ACCESS_KEY": "${env:AWS_SECRET_ACCESS_KEY}",
    "AWS_REGION": "${env:AWS_REGION}"
  }
}
```

**Mixed Static and Dynamic:**
```json
{
  "env": {
    "API_KEY": "${env:MY_API_KEY}",
    "LOG_LEVEL": "info",
    "TIMEOUT": "30000"
  }
}
```

### Workspace Variables

Use `${workspaceFolder}` to reference the current workspace:

```json
{
  "env": {
    "CONFIG_PATH": "${workspaceFolder}/.config",
    "DATA_DIR": "${workspaceFolder}/data"
  }
}
```

### Best Practices for Environment Variables

- **Never hardcode secrets** in mcp.json
- **Use descriptive names** like `STRIPE_SECRET_KEY` not `KEY`
- **Document required variables** in your POWER.md
- **Provide setup instructions** for users to configure their environment

## Timeout Settings

Timeouts control how long Kiro waits for server responses.

### Configuration

```json
{
  "mcpServers": {
    "my-server": {
      "command": "uvx",
      "args": ["package@latest"],
      "timeout": 30000
    }
  }
}
```

### Recommended Values

| Use Case | Timeout | Rationale |
|----------|---------|-----------|
| Fast operations | 10000 (10s) | Quick lookups, simple queries |
| Standard operations | 30000 (30s) | Most API calls, file operations |
| Slow operations | 60000 (60s) | Large data processing, complex queries |
| Very slow operations | 120000 (2min) | Bulk operations, long-running tasks |

### Guidelines

- **Default is 60000ms (1 minute)** if not specified
- **Set lower timeouts** for fast operations to fail quickly
- **Set higher timeouts** for operations that legitimately take time
- **Consider user experience** - long timeouts can make the agent seem unresponsive

## Auto-Approve Settings

Auto-approve allows specific tools to run without user confirmation.

### Configuration

```json
{
  "mcpServers": {
    "my-server": {
      "command": "uvx",
      "args": ["package@latest"],
      "autoApprove": ["read_file", "list_items", "get_status"]
    }
  }
}
```

### Guidelines

**Safe to auto-approve:**
- Read-only operations (list, get, read, search)
- Status checks and health checks
- Non-destructive queries

**Require approval:**
- Write operations (create, update, delete)
- Operations that cost money (API calls with billing)
- Operations that modify external state
- Operations with side effects

### Example: Read-Only Auto-Approve

```json
{
  "mcpServers": {
    "database": {
      "command": "uvx",
      "args": ["db-mcp-server@latest"],
      "autoApprove": [
        "list_tables",
        "describe_table",
        "get_schema",
        "count_rows"
      ]
    }
  }
}
```

## Disabled Tools

Disable specific tools that you don't want available:

```json
{
  "mcpServers": {
    "my-server": {
      "command": "uvx",
      "args": ["package@latest"],
      "disabledTools": ["dangerous_operation", "admin_reset"]
    }
  }
}
```

**Use cases:**
- Disable destructive operations in production
- Hide tools not relevant to your power's purpose
- Prevent accidental use of dangerous features

## Multiple Servers

Powers can include multiple MCP servers:

```json
{
  "mcpServers": {
    "database": {
      "command": "uvx",
      "args": ["db-mcp-server@latest"],
      "env": {
        "DATABASE_URL": "${env:DATABASE_URL}"
      },
      "timeout": 30000
    },
    "storage": {
      "command": "uvx",
      "args": ["storage-mcp-server@latest"],
      "env": {
        "STORAGE_BUCKET": "${env:S3_BUCKET}"
      },
      "timeout": 60000
    },
    "notifications": {
      "type": "http",
      "url": "https://api.notifications.example.com/mcp",
      "timeout": 10000
    }
  }
}
```

## Complete Examples

### Simple CLI Tool

```json
{
  "mcpServers": {
    "git-helper": {
      "command": "uvx",
      "args": ["git-mcp-server@latest"],
      "timeout": 30000,
      "autoApprove": ["status", "log", "diff", "branch_list"]
    }
  }
}
```

### API Integration with Auth

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@github/mcp-server@latest"],
      "env": {
        "GITHUB_TOKEN": "${env:GITHUB_TOKEN}",
        "GITHUB_ORG": "${env:GITHUB_ORG}"
      },
      "timeout": 45000,
      "autoApprove": ["list_repos", "get_repo", "list_issues"]
    }
  }
}
```

### Full-Featured Configuration

```json
{
  "mcpServers": {
    "primary-api": {
      "command": "uvx",
      "args": ["my-mcp-server@latest", "--verbose"],
      "env": {
        "API_KEY": "${env:PRIMARY_API_KEY}",
        "API_SECRET": "${env:PRIMARY_API_SECRET}",
        "ENVIRONMENT": "production",
        "LOG_LEVEL": "info"
      },
      "timeout": 60000,
      "autoApprove": ["list", "get", "search", "status"],
      "disabledTools": ["admin_reset", "delete_all"],
      "disabled": false
    }
  }
}
```

## Validation Checklist

Before finalizing your mcp.json:

- [ ] File is valid JSON (no trailing commas, proper quotes)
- [ ] Server names are kebab-case
- [ ] Each stdio server has `command` and `args`
- [ ] Each http server has `type: "http"` and `url`
- [ ] Each remote server has `type: "remote"` and `url`
- [ ] Environment variables use `${env:NAME}` syntax
- [ ] Timeouts are appropriate for expected operation duration
- [ ] Auto-approve only includes safe, read-only operations
- [ ] Server names match references in POWER.md frontmatter
- [ ] Required environment variables are documented in POWER.md

## Known Issues

### "This file is protected" Error When Creating mcp.json

**Symptom:** When using file write tools to create `mcp.json`, you may see:
```
"This file is protected. You cannot overwrite the contents."
```

**Reality:** This is a known Kiro bug. The file is actually written successfully despite the error message.

**Workaround:** 
1. Ignore the error - check if the file was created with `ls` or `readFile`
2. The content is usually written correctly
3. If needed, verify the file contents after the "error"

**Alternative approach using bash:**
```bash
cat > your-power/mcp.json << 'EOF'
{
  "mcpServers": {
    "server-name": {
      "command": "uvx",
      "args": ["package@latest"]
    }
  }
}
EOF
```

**Status:** This is a backend bug in Kiro's file protection logic. It incorrectly flags `mcp.json` files as protected but still writes them.

## Common Issues

### "Command not found"

**Cause:** The command (uvx, npx) is not installed or not in PATH
**Solution:** 
- For uvx: Install uv (`pip install uv` or `brew install uv`)
- For npx: Install Node.js

### "Server timeout"

**Cause:** Server takes longer than configured timeout
**Solution:** Increase timeout value or optimize server performance

### "Environment variable not set"

**Cause:** Referenced environment variable doesn't exist
**Solution:** Set the variable in your shell or .env file

### "Invalid JSON"

**Cause:** Syntax error in mcp.json
**Solution:** Validate JSON syntax (check for trailing commas, missing quotes)

## Next Steps

After configuring MCP servers:

1. **Document servers in POWER.md** → See `steering/power-md-guide.md`
2. **Create steering files** → See `steering/steering-files.md`
3. **Validate your power** → See `steering/validation.md`
4. **Test locally** → See `steering/testing.md`
5. **Publish to GitHub** → See `steering/publishing.md`
