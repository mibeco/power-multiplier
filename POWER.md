---
name: "power-multiplier"
displayName: "Power Multiplier"
description: "Create Kiro Powers with guided assistance - scaffold projects, configure MCP servers, write steering files, and validate before publishing"
keywords: ["power", "create power", "build power", "power multiplier", "multiply powers", "mcp", "steering", "kiro power", "new power"]
author: "Kiro Community"
---

# Power Multiplier

## Overview

Power Multiplier helps you create Kiro Powers from scratch. Whether you're building a simple documentation-only power, a single-tool MCP integration, or a complex power with multiple steering files, this power guides you through every step.

### What You Can Build

- **Simple Powers**: Just a POWER.md file with documentation and instructions
- **MCP Powers**: Include mcp.json to provide tools via Model Context Protocol servers
- **Full Powers**: Complete packages with steering files for workflow-specific guidance

### How It Works

1. Tell Kiro what kind of power you want to create
2. Power Multiplier activates and loads relevant steering files
3. Follow the guided workflow to scaffold, write, configure, and validate
4. Test locally, then publish to GitHub for others to install

## When to Load Steering Files

| Workflow | Steering File | When to Use |
|----------|---------------|-------------|
| Starting a new power | `steering/scaffolding.md` | Creating the initial project structure |
| Writing POWER.md | `steering/power-md-guide.md` | Crafting effective agent instructions |
| Adding MCP servers | `steering/mcp-config.md` | Configuring stdio, http, or remote servers |
| Creating steering files | `steering/steering-files.md` | Adding workflow-specific guidance |
| Validating your power | `steering/validation.md` | Checking for errors before publishing |
| Testing locally | `steering/testing.md` | Installing and testing via Powers panel |
| Publishing to GitHub | `steering/publishing.md` | Making your power available to others |

## Quick Start

### Create a New Power

Ask: "I want to create a new Kiro power called [name] that [description]"

The scaffolding workflow will:
1. Prompt for power metadata (name, displayName, description, keywords)
2. Ask what components you need (MCP servers? steering files?)
3. Generate the appropriate file structure
4. Create initial POWER.md with your metadata

### Add MCP Servers

Ask: "Add an MCP server to my power" or "Configure [server name] for my power"

The MCP configuration workflow will:
1. Determine server type (stdio, http, remote)
2. Collect required configuration (command, args, env, url)
3. Generate valid mcp.json
4. Update POWER.md with server documentation

### Validate Before Publishing

Ask: "Validate my power" or "Check if my power is ready to publish"

The validation workflow will:
1. Verify POWER.md exists with valid frontmatter
2. Check mcp.json syntax and required fields
3. Ensure steering files have proper frontmatter
4. Confirm all steering files are referenced in POWER.md
5. Report issues with specific remediation steps

## Power Structure Reference

### Required Files

```
your-power/
└── POWER.md    # Required - metadata and agent instructions
```

### Optional Files

```
your-power/
├── POWER.md           # Required
├── mcp.json           # Optional - MCP server configuration
└── steering/          # Optional - workflow guidance
    └── *.md           # Steering files
```

### POWER.md Frontmatter

```yaml
---
name: "your-power-name"           # kebab-case identifier
displayName: "Your Power Name"    # Human-readable name
description: "One-line description of what your power does"
keywords: ["keyword1", "keyword2", "keyword3"]
author: "Your Name"               # Optional
---
```

### mcp.json Structure

```json
{
  "mcpServers": {
    "server-name": {
      "command": "uvx",
      "args": ["package-name@latest"],
      "env": {
        "API_KEY": "${env:YOUR_API_KEY}"
      },
      "timeout": 30000,
      "autoApprove": []
    }
  }
}
```

### Steering File Frontmatter

```yaml
---
inclusion: always           # always | fileMatch | manual
fileMatchPattern: "*.ts"    # Required if inclusion is fileMatch
---
```

## Best Practices

### Keywords
- Include both short and long forms ("db", "database")
- Use terms users naturally say in conversation
- Include your power's name and common synonyms

### POWER.md Content
- Start with a clear overview of what the power does
- Document each MCP server's tools and usage
- Include code examples and templates
- Map workflows to steering files

### Steering Files
- Use `inclusion: always` for core guidance
- Use `inclusion: fileMatch` for file-type-specific help
- Use `inclusion: manual` for optional deep-dives
- Keep each file focused on one workflow

### MCP Servers
- Document required environment variables
- Provide setup instructions for dependencies
- Include example tool invocations
- Set appropriate timeouts
