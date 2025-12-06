---
inclusion: always
---

# Power Publishing Guide

## Overview

This guide walks you through publishing your Kiro Power to GitHub so others can discover and install it. Publishing makes your power available to the Kiro community and enables easy installation via URL.

## Publishing Workflow

Follow this sequence to publish your power:

1. **Prepare repository** - Ensure all required files are in place
2. **Create GitHub repository** - Set up public repo
3. **Push your power** - Upload files to GitHub
4. **Verify installation** - Test installing from GitHub URL
5. **Share your power** - Distribute the installation URL

## GitHub Repository Requirements

### Required Files

Your repository must contain these files at the root level:

| File | Purpose | Required |
|------|---------|----------|
| `POWER.md` | Power metadata and instructions | ✅ Yes |
| `README.md` | Human-readable documentation | Recommended |
| `LICENSE` | Usage terms (MIT recommended) | Recommended |
| `mcp.json` | MCP server configuration | If using MCP |
| `steering/` | Workflow guidance files | If using steering |

### Repository Structure

```
your-power/                    # Repository root
├── POWER.md                   # Required - Kiro reads this
├── README.md                  # For GitHub visitors
├── LICENSE                    # MIT recommended
├── mcp.json                   # Optional - MCP servers
└── steering/                  # Optional - steering files
    ├── workflow1.md
    └── workflow2.md
```

### Repository Settings

| Setting | Requirement |
|---------|-------------|
| Visibility | **Public** (required for installation) |
| Default branch | `main` or `master` |
| Files at root | POWER.md must be at repository root |

## Preparing for Publication

### Step 1: Final Validation

Before publishing, run complete validation:

```bash
# Validate all files
./validate-power.sh

# Or manually check:
# 1. POWER.md has valid frontmatter
# 2. mcp.json is valid JSON (if present)
# 3. Steering files have frontmatter (if present)
# 4. All cross-references are correct
```

See `steering/validation.md` for detailed validation steps.

### Step 2: Optimize for Discoverability

#### Choosing Effective Keywords

Keywords determine when your power activates. Choose wisely:

**Good Keywords:**
- Common terms users naturally say
- Both short and long forms (`db`, `database`)
- Action phrases (`create database`, `query data`)
- Your power's name and variations
- Related technologies and concepts

**Avoid:**
- Generic terms (`help`, `code`, `fix`)
- Highly technical jargon
- Single letters
- Terms that overlap with other powers



**Keyword Examples by Power Type:**

| Power Type | Good Keywords |
|------------|---------------|
| Database | `database`, `db`, `sql`, `query`, `schema`, `postgres` |
| API Integration | `stripe`, `payment`, `checkout`, `billing`, `subscription` |
| DevOps | `deploy`, `kubernetes`, `k8s`, `docker`, `container` |
| Documentation | `docs`, `readme`, `documentation`, `api docs` |

#### Writing Clear Descriptions

Your description appears in the Powers panel and search results:

**Good descriptions:**
- One clear sentence
- States what the power does
- Mentions key capabilities
- Uses searchable terms

**Examples:**

```yaml
# ✅ Good
description: "Integrate Stripe payments with guided checkout flows and subscription management"

# ✅ Good  
description: "Write and validate Kubernetes manifests with best practices and troubleshooting"

# ❌ Too vague
description: "Helps with payments"

# ❌ Too long
description: "This power helps developers integrate payment processing into their applications using the Stripe API with support for one-time payments, subscriptions, invoicing, and more"
```

### Step 3: Write README.md

Create a README for humans browsing GitHub:

```markdown
# Your Power Name

Brief description of what your power does.

## Installation

Install via Kiro Powers panel:

1. Open Kiro
2. Go to Powers panel
3. Click "Add power from GitHub"
4. Enter: `https://github.com/your-username/your-power-name`

## Features

- Feature 1: Description
- Feature 2: Description
- Feature 3: Description

## Keywords

This power activates when you mention:
- keyword1
- keyword2
- keyword3

## Usage Examples

### Example 1: [Use Case]

```
[Example prompt that activates the power]
```

### Example 2: [Use Case]

```
[Another example prompt]
```

## Requirements

[Any prerequisites like API keys, installed tools, etc.]

## License

MIT
```

### Step 4: Add LICENSE

Use MIT license (recommended for open source):

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

## Creating GitHub Repository

### Option 1: GitHub Web Interface

1. Go to [github.com/new](https://github.com/new)
2. Enter repository name (should match power `name`)
3. Set visibility to **Public**
4. Don't initialize with README (you have your own)
5. Click "Create repository"
6. Follow the push instructions

### Option 2: GitHub CLI

```bash
# Install GitHub CLI if needed
# brew install gh  (macOS)
# sudo apt install gh  (Ubuntu)

# Authenticate
gh auth login

# Create repository
cd your-power-directory
gh repo create your-power-name --public --source=. --push
```

### Option 3: Git Commands

```bash
# Initialize if not already done
cd your-power-directory
git init

# Add all files
git add .
git commit -m "Initial power release"

# Create repo on GitHub first, then:
git remote add origin https://github.com/your-username/your-power-name.git
git branch -M main
git push -u origin main
```

## Installation URL Format

Once published, your power can be installed using:

```
https://github.com/your-username/your-power-name
```

**Examples:**
- `https://github.com/stripe/stripe-mcp-power`
- `https://github.com/kirodotdev/cloud-architect`
- `https://github.com/your-username/my-awesome-power`

### URL Requirements

| Requirement | Details |
|-------------|---------|
| Protocol | `https://` (required) |
| Host | `github.com` |
| Path | `username/repository-name` |
| Branch | Uses default branch (main/master) |
| POWER.md | Must be at repository root |

## Verifying Publication

### Step 1: Test Installation from URL

1. Uninstall your local power (if installed)
2. Open Kiro Powers panel
3. Click "Add power from GitHub"
4. Enter your GitHub URL
5. Verify power installs correctly

### Step 2: Verify All Features

After installing from GitHub:

- [ ] Power appears in Powers panel
- [ ] Display name is correct
- [ ] Description is correct
- [ ] Keywords trigger activation
- [ ] MCP servers connect (if applicable)
- [ ] Steering files load (if applicable)

### Step 3: Test as New User

Ask someone else to install and test:

1. Share your GitHub URL
2. Have them install the power
3. Get feedback on:
   - Installation experience
   - Activation reliability
   - Feature functionality
   - Documentation clarity

## Sharing Your Power

### Installation Instructions

Share these instructions with users:

```markdown
## Install [Your Power Name]

1. Open Kiro
2. Click the Powers icon in the sidebar
3. Click "Add power from GitHub"
4. Paste: `https://github.com/your-username/your-power-name`
5. Click Install

The power will activate when you mention: [keywords]
```

### Sharing Channels

- GitHub README
- Social media
- Developer communities
- Kiro community forums
- Blog posts

## Updating Published Powers

### Making Updates

1. Make changes locally
2. Test changes thoroughly
3. Commit and push to GitHub:

```bash
git add .
git commit -m "feat: add new feature"
git push origin main
```

### User Updates

Users with your power installed can update by:

1. Opening Powers panel
2. Finding your power
3. Clicking "Update" (if available)

Or reinstalling:
1. Remove the power
2. Add from GitHub URL again

### Versioning (Optional)

Consider using git tags for versions:

```bash
git tag v1.0.0
git push origin v1.0.0
```

## Publication Checklist

Before announcing your power:

### Required
- [ ] POWER.md at repository root
- [ ] Valid frontmatter with all required fields
- [ ] Repository is public
- [ ] Installation from URL works

### Recommended
- [ ] README.md with installation instructions
- [ ] LICENSE file (MIT recommended)
- [ ] Clear, searchable description
- [ ] 5-8 relevant keywords
- [ ] Tested by someone other than you

### If Using MCP Servers
- [ ] mcp.json is valid
- [ ] Server dependencies documented
- [ ] Environment variables documented
- [ ] Servers connect after fresh install

### If Using Steering Files
- [ ] All files have valid frontmatter
- [ ] Files referenced in POWER.md
- [ ] FileMatch patterns documented

## Common Publishing Issues

### Repository Not Found

**Cause:** Repository is private or URL is wrong

**Solution:**
- Verify repository is public
- Check URL spelling
- Ensure POWER.md is at root

### POWER.md Not Found

**Cause:** File not at repository root

**Solution:**
- Move POWER.md to root directory
- Check file name is exactly `POWER.md`
- Verify file was committed and pushed

### Invalid Frontmatter

**Cause:** YAML syntax error in POWER.md

**Solution:**
- Validate YAML syntax
- Check `---` delimiters
- Verify required fields present

### MCP Server Fails After Install

**Cause:** Missing dependencies or environment variables

**Solution:**
- Document all requirements in README
- Provide setup instructions
- List required environment variables

## Best Practices

### For Discoverability

1. **Use descriptive name** - Make it clear what the power does
2. **Write searchable description** - Include key terms
3. **Choose relevant keywords** - Think like your users
4. **Add comprehensive README** - Help users understand value

### For Reliability

1. **Test thoroughly** - Before and after publishing
2. **Document requirements** - Dependencies, API keys, etc.
3. **Handle errors gracefully** - Provide helpful messages
4. **Keep it focused** - One power, one purpose

### For Maintenance

1. **Use semantic versioning** - v1.0.0, v1.1.0, v2.0.0
2. **Document changes** - Keep a CHANGELOG
3. **Respond to issues** - Monitor GitHub issues
4. **Update regularly** - Keep dependencies current

## Next Steps

After publishing:

1. **Monitor usage** - Watch for issues and feedback
2. **Iterate based on feedback** - Improve based on user input
3. **Share with community** - Help others discover your power
4. **Consider contributing** - Share learnings with other power creators
