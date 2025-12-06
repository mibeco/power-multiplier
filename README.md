# Power Multiplier

A Kiro Power that helps you create other Kiro Powers with guided assistance.

## What is this?

Power Multiplier provides step-by-step guidance for:

- **Scaffolding** new power projects with the correct file structure
- **Writing** effective POWER.md content that instructs the AI agent
- **Configuring** MCP servers (stdio, http, remote)
- **Creating** steering files for workflow-specific guidance
- **Validating** your power before publishing
- **Testing** locally via the Kiro Powers panel
- **Publishing** to GitHub for others to install

## Installation

### From GitHub

1. Open Kiro
2. Go to the Powers panel
3. Click "Add power from GitHub"
4. Paste: `https://github.com/mibeco/power-multiplier`

### From Local Path (for development)

1. Clone this repository
2. Open Kiro Powers panel
3. Click "Add power from local path"
4. Select the `power-multiplier` directory

## Usage

Once installed, just ask Kiro about creating powers:

- "I want to create a new Kiro power"
- "Help me build a power for [your use case]"
- "How do I configure MCP servers for my power?"
- "Validate my power before publishing"

The Power Multiplier will activate automatically based on keywords and guide you through the process.

## Power Structure

This power includes:

```
power-multiplier/
├── POWER.md              # Main instructions for Kiro
├── steering/             # Workflow-specific guides
│   ├── scaffolding.md    # Project setup
│   ├── power-md-guide.md # Writing POWER.md
│   ├── mcp-config.md     # MCP configuration
│   ├── steering-files.md # Creating steering files
│   ├── validation.md     # Validation checklist
│   ├── testing.md        # Local testing
│   └── publishing.md     # Publication guide
├── examples/             # Reference implementations
│   ├── simple-power/     # POWER.md only
│   ├── mcp-power/        # With mcp.json
│   └── full-power/       # With steering files
├── README.md             # This file
└── LICENSE               # MIT License
```

## License

MIT License - see [LICENSE](LICENSE) for details.
