---
inclusion: always
---

# Power Testing Guide

## Overview

This guide helps you test your Kiro Power locally before publishing. Local testing ensures your power activates correctly, MCP servers connect properly, and steering files load as expected. Testing catches issues early and builds confidence before sharing your power with others.

## Testing Workflow

Follow this sequence to thoroughly test your power:

1. **Install locally** - Add power from local path
2. **Test activation** - Verify keywords trigger the power
3. **Test MCP servers** - Confirm tools are available (if applicable)
4. **Test steering files** - Verify context loads appropriately (if applicable)
5. **Iterate and fix** - Address any issues found

## Local Installation

### Step 1: Open Powers Panel

1. Open Kiro
2. Click the Powers icon in the sidebar (or use Command Palette: "Kiro: Open Powers")
3. You'll see your installed powers and options to add more

### Step 2: Add Power from Local Path

1. Click "Add power from local folder" or the folder icon
2. Navigate to your power's root directory (where POWER.md is located)
3. Select the folder
4. Kiro will validate and install the power

### Step 3: Verify Installation

After installation, your power should appear in the Powers panel with:
- Display name from frontmatter
- Description from frontmatter
- Status indicator (enabled/disabled)

### Troubleshooting Installation

| Issue | Cause | Solution |
|-------|-------|----------|
| Power doesn't appear | Invalid POWER.md | Run validation (see `steering/validation.md`) |
| "Invalid frontmatter" error | YAML syntax error | Check `---` delimiters and field formatting |
| "Missing required field" | Incomplete frontmatter | Add missing name/displayName/description/keywords |

## Testing Activation

### Understanding Keyword Activation

Kiro activates powers when conversation messages contain keywords from the power's frontmatter. Test that your keywords trigger activation reliably.

### Generating Test Prompts

Create test prompts that include your keywords naturally:

```yaml
# If your keywords are:
keywords: ["stripe", "payment", "checkout", "billing"]

# Test prompts should include these terms:
```

**Example Test Prompts:**

| Keyword | Test Prompt |
|---------|-------------|
| `stripe` | "Help me integrate Stripe into my app" |
| `payment` | "I need to add payment processing" |
| `checkout` | "How do I build a checkout flow?" |
| `billing` | "Set up billing for subscriptions" |

### Test Prompt Generation Guidelines

Good test prompts:
- Use keywords in natural sentences
- Represent real user requests
- Cover different keyword variations
- Test both single keywords and combinations



### Test Prompt Template

For each keyword in your power, create at least one test prompt:

```markdown
## Test Prompts for [Your Power Name]

### Keywords: [keyword1, keyword2, keyword3]

| # | Prompt | Expected Behavior |
|---|--------|-------------------|
| 1 | "[Natural sentence with keyword1]" | Power activates, relevant context loads |
| 2 | "[Natural sentence with keyword2]" | Power activates, relevant context loads |
| 3 | "[Sentence combining keyword1 and keyword3]" | Power activates with full context |
```

### Verifying Activation

When testing activation:

1. **Start a new chat** - Clear context from previous tests
2. **Send test prompt** - Use one of your generated prompts
3. **Check for activation** - Look for power indicator in response
4. **Verify context** - Ensure relevant guidance appears in response

**Signs of successful activation:**
- Response references your power's domain knowledge
- MCP tools are available (if applicable)
- Steering file guidance appears in context

**Signs of failed activation:**
- Generic response without domain knowledge
- "I don't have access to..." messages
- MCP tools not recognized

## Testing MCP Servers

If your power includes MCP servers, test their connectivity and tool availability.

### Step 1: Verify Server Status

After installing your power:

1. Open the MCP Servers view in Kiro
2. Find your power's servers
3. Check status indicators:
   - 🟢 Green = Connected
   - 🔴 Red = Disconnected/Error
   - 🟡 Yellow = Connecting

### Step 2: Test Tool Availability

Send a prompt that should use your MCP tools:

```
"List the available tools from [your-server-name]"
```

Or ask Kiro to use a specific tool:

```
"Use the [tool-name] tool to [action]"
```

### Step 3: Test Tool Execution

For each tool in your MCP server:

1. Craft a prompt that would invoke the tool
2. Send the prompt
3. Verify the tool executes correctly
4. Check the response for expected output

### MCP Server Troubleshooting

| Issue | Cause | Solution |
|-------|-------|----------|
| Server won't connect | Command not found | Install required package (uvx, npx, etc.) |
| Server disconnects | Timeout too short | Increase `timeout` in mcp.json |
| Tools not available | Server not started | Check server logs, restart Kiro |
| Tool execution fails | Missing env vars | Set required environment variables |
| "Permission denied" | Auth required | Configure API keys in env |

### Checking Server Logs

If a server fails to connect:

1. Open Kiro's Output panel
2. Select "MCP Servers" from dropdown
3. Look for error messages
4. Common errors:
   - `ENOENT` - Command not found
   - `ECONNREFUSED` - Server not responding
   - `ETIMEDOUT` - Connection timeout

### Environment Variable Testing

If your server requires environment variables:

```json
{
  "mcpServers": {
    "my-server": {
      "command": "uvx",
      "args": ["my-package"],
      "env": {
        "API_KEY": "${env:MY_API_KEY}"
      }
    }
  }
}
```

Test that variables are set:

```bash
# Check if variable is set
echo $MY_API_KEY

# Set temporarily for testing
export MY_API_KEY="your-test-key"
```

## Testing Steering Files

If your power includes steering files, verify they load in the correct contexts.

### Testing "always" Inclusion

Steering files with `inclusion: always` should load whenever the power activates:

1. Activate your power with any keyword
2. Ask a question related to the steering file's topic
3. Verify the response includes guidance from that file

### Testing "fileMatch" Inclusion

Steering files with `inclusion: fileMatch` should load when matching files are open:

1. Open a file matching your `fileMatchPattern`
2. Activate your power
3. Verify file-specific guidance appears

**Example:**
```yaml
# steering/typescript.md
---
inclusion: fileMatch
fileMatchPattern: "**/*.ts"
---
```

Test by:
1. Opening a `.ts` file
2. Asking a question about TypeScript
3. Verifying TypeScript-specific guidance appears

### Testing "manual" Inclusion

Steering files with `inclusion: manual` load only when explicitly referenced:

1. Activate your power
2. Reference the steering file: "Use the [steering-file-name] guide"
3. Verify the guidance loads

### Steering File Troubleshooting

| Issue | Cause | Solution |
|-------|-------|----------|
| File doesn't load | Invalid frontmatter | Check `inclusion` field syntax |
| FileMatch not working | Wrong pattern | Test glob pattern matches your files |
| Content not appearing | File not referenced | Add to POWER.md steering table |
| Partial content | File too large | Split into smaller focused files |

## Common Testing Issues

### Power Not Activating

**Symptoms:**
- Keywords don't trigger the power
- Generic responses without domain knowledge

**Diagnosis:**
1. Check power is installed and enabled
2. Verify keywords in frontmatter
3. Test with exact keyword match
4. Check for typos in keywords

**Solutions:**
- Reinstall the power
- Add more common keyword variations
- Use phrases users actually say

### MCP Server Connection Failures

**Symptoms:**
- Server shows red status
- Tools not available
- Timeout errors

**Diagnosis:**
1. Check command is installed (`which uvx`, `which npx`)
2. Verify package name is correct
3. Test command manually in terminal
4. Check network connectivity for remote servers

**Solutions:**
```bash
# Install uvx if missing
pip install uv

# Install npx if missing
npm install -g npx

# Test command manually
uvx package-name@latest --help
```

### Steering Files Not Loading

**Symptoms:**
- Context missing expected guidance
- FileMatch patterns not triggering

**Diagnosis:**
1. Check frontmatter syntax
2. Verify inclusion type is correct
3. Test glob patterns manually
4. Ensure files are referenced in POWER.md

**Solutions:**
- Fix YAML frontmatter syntax
- Adjust fileMatchPattern for your files
- Add missing references to POWER.md

### Environment Variable Issues

**Symptoms:**
- "API key not found" errors
- Authentication failures
- Server starts but tools fail

**Diagnosis:**
1. Check variable is set in shell
2. Verify variable name matches mcp.json
3. Test API key validity

**Solutions:**
```bash
# Set variable in current shell
export API_KEY="your-key"

# Add to shell profile for persistence
echo 'export API_KEY="your-key"' >> ~/.bashrc
source ~/.bashrc
```

## Testing Checklist

Use this checklist to ensure thorough testing:

### Installation
- [ ] Power installs without errors
- [ ] Power appears in Powers panel
- [ ] Display name and description are correct

### Activation
- [ ] Each keyword triggers activation
- [ ] Natural prompts activate the power
- [ ] Combined keywords work correctly

### MCP Servers (if applicable)
- [ ] All servers show connected status
- [ ] Tools are listed and available
- [ ] Tool execution returns expected results
- [ ] Environment variables are working

### Steering Files (if applicable)
- [ ] "always" files load on activation
- [ ] "fileMatch" files load for matching files
- [ ] "manual" files load when referenced
- [ ] Content appears in responses

### Edge Cases
- [ ] Power works in new chat sessions
- [ ] Power works alongside other powers
- [ ] Errors are handled gracefully

## Iterating on Issues

When testing reveals issues:

1. **Document the issue** - Note exact symptoms and steps to reproduce
2. **Identify the cause** - Use troubleshooting guides above
3. **Make targeted fixes** - Change one thing at a time
4. **Retest** - Verify the fix works
5. **Regression test** - Ensure fix didn't break other functionality

### Quick Fix Reference

| Issue | Quick Fix |
|-------|-----------|
| Keywords not working | Add more variations, use common phrases |
| Server not connecting | Check command installation, increase timeout |
| Steering not loading | Fix frontmatter, check inclusion type |
| Tools failing | Set environment variables, check API keys |
| Slow activation | Reduce steering file size, optimize content |

## End-to-End Test: Create a Test Power

The best way to validate the Power Multiplier is to use it to create a simple power from scratch.

### Test 1: Create a Simple "Hello" Power

1. **Start a new chat** and say:
   ```
   "Create a new power called hello-world that greets users"
   ```

2. **Expected behavior:**
   - Power Multiplier activates
   - Prompts for power metadata (name, displayName, description, keywords)
   - Generates POWER.md with valid frontmatter
   - Creates appropriate directory structure

3. **Verify the output:**
   - Check that `hello-world/POWER.md` was created
   - Verify frontmatter has all required fields
   - Confirm the content is relevant to greeting users

### Test 2: Add MCP Server Configuration

1. **Continue the conversation:**
   ```
   "Add an MCP server to my hello-world power"
   ```

2. **Expected behavior:**
   - Prompts for server type (stdio, http, remote)
   - Collects required configuration
   - Generates valid mcp.json

3. **Verify the output:**
   - Check that `hello-world/mcp.json` was created
   - Validate JSON syntax
   - Confirm server configuration is complete

### Test 3: Create a Steering File

1. **Continue the conversation:**
   ```
   "Create a steering file for greeting customization"
   ```

2. **Expected behavior:**
   - Prompts for inclusion type
   - Generates steering file with valid frontmatter
   - Updates POWER.md with steering file reference

3. **Verify the output:**
   - Check that `hello-world/steering/customization.md` was created
   - Verify frontmatter has `inclusion` field
   - Confirm POWER.md references the new file

### Test 4: Validate the Power

1. **Ask for validation:**
   ```
   "Validate my hello-world power"
   ```

2. **Expected behavior:**
   - Checks POWER.md frontmatter
   - Validates mcp.json syntax
   - Verifies steering file frontmatter
   - Reports any issues with remediation steps

3. **Verify the output:**
   - All checks should pass
   - No errors reported

### Test 5: Test Different Activation Keywords

Try these prompts to verify keyword activation:

| Prompt | Should Activate? |
|--------|------------------|
| "I want to create a new Kiro power" | ✅ Yes |
| "Help me build a power" | ✅ Yes |
| "How do I configure MCP servers?" | ✅ Yes |
| "What's the weather today?" | ❌ No |
| "Validate my power" | ✅ Yes |
| "How do I publish my power?" | ✅ Yes |

### Test 6: Verify Steering Files Load

For each steering file, verify it loads when relevant:

| Topic | Expected Steering File |
|-------|----------------------|
| Creating a new power | `scaffolding.md` |
| Writing POWER.md | `power-md-guide.md` |
| MCP configuration | `mcp-config.md` |
| Steering files | `steering-files.md` |
| Validation | `validation.md` |
| Testing | `testing.md` |
| Publishing | `publishing.md` |

## Next Steps

After successful testing:

1. **Document any quirks** - Note special requirements in README
2. **Prepare for publishing** → See `steering/publishing.md`
3. **Share with testers** - Get feedback from others before public release
