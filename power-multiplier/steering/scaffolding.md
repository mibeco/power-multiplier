---
inclusion: always
---

# Power Scaffolding Guide

## Overview

This guide walks you through creating a new Kiro Power from scratch. You'll set up the directory structure, create required files, and configure your power based on its complexity level.

## Power Structure Options

Choose the structure that matches your power's needs:

### Simple Power (Documentation Only)

Best for: Providing context, guidelines, or domain knowledge without tools.

```
your-power/
├── POWER.md      # Required - metadata and instructions
├── README.md     # Optional - GitHub documentation
└── LICENSE       # Optional - MIT recommended
```

**Use when:**
- Your power provides expertise or context (style guides, best practices)
- No external tools or APIs are needed
- All guidance fits in POWER.md or inline steering

### MCP Power (With Tools)

Best for: Integrating external tools, APIs, or services.

```
your-power/
├── POWER.md      # Required - metadata and instructions
├── mcp.json      # Required - MCP server configuration
├── README.md     # Optional - GitHub documentation
└── LICENSE       # Optional - MIT recommended
```

**Use when:**
- Your power wraps an existing MCP server (uvx, npx packages)
- You need to provide tools to the AI agent
- External API integration is required

### Full Power (With Steering Files)

Best for: Complex workflows requiring context-specific guidance.

```
your-power/
├── POWER.md           # Required - metadata and instructions
├── mcp.json           # Optional - MCP server configuration
├── steering/          # Required - workflow guidance
│   ├── workflow1.md   # Steering files for specific workflows
│   └── workflow2.md
├── README.md          # Optional - GitHub documentation
└── LICENSE            # Optional - MIT recommended
```

**Use when:**
- Your power supports multiple distinct workflows
- Different file types need different guidance
- Deep-dive documentation would clutter POWER.md

## Step-by-Step Scaffolding

### Step 1: Create Directory Structure

```bash
# Create power directory
mkdir your-power-name
cd your-power-name

# Initialize git (recommended)
git init

# Create steering directory if needed
mkdir steering
```

### Step 2: Gather Power Metadata

Before creating files, collect this information:

| Field | Description | Example |
|-------|-------------|---------|
| `name` | kebab-case identifier | `my-awesome-power` |
| `displayName` | Human-readable name | `My Awesome Power` |
| `description` | One-line summary | `Helps developers do X with Y` |
| `keywords` | Activation triggers | `["x", "y", "do x", "help with y"]` |
| `author` | Your name (optional) | `Your Name` |

### Step 3: Create POWER.md

Use this template as your starting point:

```markdown
---
name: "your-power-name"
displayName: "Your Power Name"
description: "One-line description of what your power does"
keywords: ["keyword1", "keyword2", "action phrase"]
author: "Your Name"
---

# Your Power Name

## Overview

[Describe what your power does and who it's for]

### Key Capabilities

- [Capability 1]
- [Capability 2]
- [Capability 3]

## Getting Started

[Quick start instructions for users]

## Best Practices

[Guidelines for effective use]
```

### Step 4: Create mcp.json (If Needed)

For powers with MCP servers:

```json
{
  "mcpServers": {
    "server-name": {
      "command": "uvx",
      "args": ["package-name@latest"],
      "env": {},
      "timeout": 30000
    }
  }
}
```

### Step 5: Create Steering Files (If Needed)

For each workflow, create a steering file:

```markdown
---
inclusion: always
---

# Workflow Name

## Overview

[What this workflow accomplishes]

## Steps

1. [Step 1]
2. [Step 2]
3. [Step 3]

## Examples

[Code examples and templates]

## Common Issues

[Troubleshooting guidance]
```

### Step 6: Create README.md

For GitHub visibility:

```markdown
# Your Power Name

[Description for humans browsing GitHub]

## Installation

Install via Kiro Powers panel:
1. Open Kiro
2. Go to Powers panel
3. Click "Add power from GitHub"
4. Enter: `https://github.com/your-username/your-power-name`

## Features

- [Feature 1]
- [Feature 2]

## Usage

[How to use the power]

## License

MIT
```

### Step 7: Create LICENSE

Use MIT license (recommended):

```
MIT License

Copyright (c) [year] [your name]

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

## POWER.md Frontmatter Templates

### Minimal Frontmatter

```yaml
---
name: "power-name"
displayName: "Power Name"
description: "What this power does"
keywords: ["keyword1", "keyword2"]
---
```

### Full Frontmatter

```yaml
---
name: "power-name"
displayName: "Power Name"
description: "What this power does"
keywords: ["keyword1", "keyword2", "action phrase", "related term"]
author: "Your Name"
---
```

### With MCP Server References

```yaml
---
name: "power-name"
displayName: "Power Name"
description: "What this power does"
keywords: ["keyword1", "keyword2"]
author: "Your Name"
mcpServers:
  - server-name
---
```

## Keyword Selection Tips

Good keywords:
- Are terms users naturally say ("create database", "deploy")
- Include both short and long forms ("db", "database")
- Cover common synonyms ("deploy", "publish", "release")
- Include your power's name

Avoid:
- Generic terms that would over-trigger ("help", "code")
- Highly technical jargon users rarely type
- Single letters or very short abbreviations

## Validation Checklist

Before proceeding, verify:

- [ ] `name` is kebab-case with no spaces
- [ ] `displayName` is human-readable
- [ ] `description` is one clear sentence
- [ ] `keywords` array has 3-8 relevant terms
- [ ] POWER.md has valid YAML frontmatter (between `---` markers)
- [ ] If using mcp.json, it's valid JSON
- [ ] If using steering files, each has frontmatter with `inclusion` field
- [ ] All steering files are referenced in POWER.md

## Next Steps

After scaffolding:

1. **Write POWER.md content** → See `steering/power-md-guide.md`
2. **Configure MCP servers** → See `steering/mcp-config.md`
3. **Create steering files** → See `steering/steering-files.md`
4. **Validate your power** → See `steering/validation.md`
5. **Test locally** → See `steering/testing.md`
6. **Publish to GitHub** → See `steering/publishing.md`
