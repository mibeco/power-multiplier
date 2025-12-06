# Power Multiplier

A Kiro Power that helps you create other Kiro Powers with guided assistance.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## What is this?

Power Multiplier provides step-by-step guidance for building Kiro Powers. Whether you're creating a simple documentation power or a complex integration with MCP servers and steering files, this power guides you through every step.

### Features

- **Scaffolding** - Generate new power projects with the correct file structure
- **POWER.md Writing** - Craft effective agent instructions with templates and examples
- **MCP Configuration** - Set up stdio, http, or remote MCP servers
- **Steering Files** - Create workflow-specific guidance that loads dynamically
- **Validation** - Check your power for errors before publishing
- **Local Testing** - Test via the Kiro Powers panel
- **Publishing** - Share your power on GitHub for others to install

## Installation

### From GitHub (Recommended)

1. Open Kiro
2. Go to the Powers panel (click the Powers icon in the sidebar)
3. Click "Add power from GitHub"
4. Paste: `https://github.com/mibeco/power-multiplier`
5. Click Install

### From Local Path (for development)

1. Clone this repository:
   ```bash
   git clone https://github.com/mibeco/power-multiplier.git
   ```
2. Open Kiro Powers panel
3. Click "Add power from local path"
4. Select the `power-multiplier` directory

## Quick Start

Once installed, just ask Kiro about creating powers. The Power Multiplier activates automatically based on keywords.

### Create a New Power

```
"I want to create a new Kiro power called weather-helper that provides weather forecasts"
```

### Add MCP Servers

```
"Add an MCP server to my power"
"Configure a stdio server for my power"
```

### Validate Before Publishing

```
"Validate my power"
"Check if my power is ready to publish"
```

### Get Help

```
"How do I write a good POWER.md?"
"What should I include in my steering files?"
"How do I publish my power to GitHub?"
```

## Keywords

The power activates when you mention:
- `power`, `create power`, `build power`
- `power multiplier`, `multiply powers`
- `mcp`, `steering`, `kiro power`, `new power`

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

## Examples

The `examples/` directory contains reference implementations for different power types:

### Simple Power (`examples/simple-power/`)
A minimal power with just POWER.md - perfect for documentation-only powers that provide context and instructions without tools.

### MCP Power (`examples/mcp-power/`)
A power with POWER.md and mcp.json - demonstrates how to integrate MCP servers that provide tools to the AI agent.

### Full Power (`examples/full-power/`)
A complete power with POWER.md, mcp.json, and steering files - shows the full pattern for complex powers with workflow-specific guidance.

## What Can You Build?

| Power Type | Files | Use Case |
|------------|-------|----------|
| Simple | POWER.md only | Documentation, context, instructions |
| MCP | POWER.md + mcp.json | Tool integrations (APIs, CLIs, services) |
| Full | POWER.md + mcp.json + steering/ | Complex workflows with dynamic guidance |

## Testing

After installing the power (either from GitHub or local path), verify it works correctly:

### 1. Test Activation

Open a new Kiro chat and try these prompts to verify the power activates:

- "I want to create a new Kiro power"
- "Help me build a power"
- "How do I configure MCP servers for my power?"

You should see the Power Multiplier activate and provide guided assistance.

### 2. Test Steering Files

Verify the appropriate steering files load for different workflows:

| Prompt | Expected Steering File |
|--------|----------------------|
| "Create a new power project" | `scaffolding.md` |
| "Help me write my POWER.md" | `power-md-guide.md` |
| "Add an MCP server" | `mcp-config.md` |
| "Create a steering file" | `steering-files.md` |
| "Validate my power" | `validation.md` |
| "Test my power locally" | `testing.md` |
| "Publish my power" | `publishing.md` |

### 3. Test End-to-End

Try creating a simple test power from scratch:

1. Ask: "Create a new power called test-power that says hello"
2. Follow the scaffolding workflow
3. Ask: "Validate my power"
4. Verify validation passes

## Contributing

Contributions are welcome! If you have ideas for improving the Power Multiplier:

1. Fork this repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

## Resources

- [Kiro Documentation](https://kiro.dev/docs)
- [MCP Protocol](https://modelcontextprotocol.io)
- [Official Kiro Powers](https://github.com/kirodotdev/powers)

## License

MIT License - see [LICENSE](LICENSE) for details.
