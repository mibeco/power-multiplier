# Power Multiplier

A Kiro Power that helps you create other Kiro Powers with guided assistance.

## Installation

Install via the Kiro Powers panel using this URL:

```
https://github.com/mibeco/power-multiplier/tree/main/power-multiplier
```

1. Open Kiro
2. Go to the Powers panel (click the Powers icon in the sidebar)
3. Click "Add power from GitHub"
4. Paste the URL above
5. Click Install

## What It Does

Power Multiplier provides step-by-step guidance for building Kiro Powers:

- **Scaffolding** - Generate new power projects with the correct file structure
- **POWER.md Writing** - Craft effective agent instructions with templates
- **MCP Configuration** - Set up stdio, http, or remote MCP servers
- **Steering Files** - Create workflow-specific guidance
- **Validation** - Check your power for errors before publishing
- **Publishing** - Share your power on GitHub

## Usage

Once installed, ask Kiro about creating powers:

- "I want to create a new Kiro power"
- "Help me build a power for [your use case]"
- "How do I configure MCP servers?"
- "Validate my power before publishing"

## Keywords

The power activates when you mention:
`power`, `create power`, `build power`, `power multiplier`, `mcp`, `steering`, `kiro power`

## Power Structure

```
power-multiplier/
└── power-multiplier/     # The actual power
    ├── POWER.md          # Main instructions for Kiro
    └── steering/         # Workflow-specific guides
        ├── scaffolding.md
        ├── power-md-guide.md
        ├── mcp-config.md
        ├── steering-files.md
        ├── validation.md
        ├── testing.md
        └── publishing.md
```

## Important: GitHub URL Format

When publishing your own power, note that Kiro requires powers to be in a subdirectory. The URL format is:

```
https://github.com/{owner}/{repo}/tree/main/{power-name}
```

Not just `https://github.com/{owner}/{repo}`
