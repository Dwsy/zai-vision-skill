---
name: zai-vision
description: Dynamic access to zai-vision MCP server (8 tools, transport: stdio)
---

# zai-vision Skill

This skill provides dynamic access to the zai-vision MCP server with progressive disclosure loading.

## Transport Protocol

**Protocol**: Standard Input/Output (stdio)



## Context Efficiency

Traditional MCP approach:
- All 8 tools loaded at startup
- Estimated context: 4000 tokens

This skill approach:
- Metadata only: ~150 tokens
- Full instructions (when used): ~5k tokens
- Tool execution: 0 tokens (runs externally)

## Available Tools

**`ui_to_artifact`** - Convert UI screenshots into various artifacts: code, prompts, design specifications, or descriptions.
**`extract_text_from_screenshot`** - Extract and recognize text from screenshots using advanced OCR capabilities.
**`diagnose_error_screenshot`** - Diagnose and analyze error messages, stack traces, and exception screenshots.
**`understand_technical_diagram`** - Analyze and explain technical diagrams including architecture diagrams, flowcharts, UML, ER diagrams, and system design diagrams.
**`analyze_data_visualization`** - Analyze data visualizations, charts, graphs, and dashboards to extract insights and trends.
**`ui_diff_check`** - Compare two UI screenshots to identify visual differences and implementation discrepancies.
**`analyze_image`** - General-purpose image analysis for scenarios not covered by specialized tools.
**`analyze_video`** - Analyze video content using advanced AI vision models.

## Usage Pattern

When the user's request matches this skill's capabilities:

**Step 1: Identify the right tool** from the list above

**Step 2: Generate a tool call** in this JSON format:

```json
{
  "tool": "tool_name",
  "arguments": {
    "param1": "value1",
    "param2": "value2"
  }
}
```

**Step 3: Execute via bash:**

```bash
cd $SKILL_DIR
uv run executor.py --call 'YOUR_JSON_HERE'
```

⚠️ **重要**: Replace $SKILL_DIR with the actual discovered path of this skill directory.

## Getting Tool Details

If you need detailed information about a specific tool's parameters:

```bash
cd $SKILL_DIR
uv run executor.py --describe tool_name
```

## Examples

### Example 1: List all tools

```bash
cd $SKILL_DIR
uv run executor.py --list
```

### Example 2: Describe a tool

```bash
cd $SKILL_DIR
uv run executor.py --describe tool_name
```

### Example 3: Call a tool

```bash
cd $SKILL_DIR
uv run executor.py --call '{"tool": "tool_name", "arguments": {"param1": "value"}}'
```

### Example 4: Show status

```bash
cd $SKILL_DIR
uv run executor.py --status
```

### Example 5: Show statistics

```bash
cd $SKILL_DIR
uv run executor.py --stats
```

### Example 6: Show recent logs

```bash
cd $SKILL_DIR
uv run executor.py --logs 50
```

### Example 7: Filter logs by tool

```bash
cd $SKILL_DIR
uv run executor.py --logs 100 --tool tool_name
```

### Example 8: Reset statistics

```bash
cd $SKILL_DIR
uv run executor.py --reset-stats
```

## Error Handling

If the executor returns an error:
- Check the tool name is correct
- Verify required arguments are provided
- Ensure the MCP server is accessible
- Check API keys in mcp-config.json

## Performance Notes

Context usage comparison:

| Scenario | MCP (preload) | Skill (dynamic) |
|----------|---------------|-----------------|
| Idle | 4000 tokens | 150 tokens |
| Active | 4000 tokens | 5k tokens |
| Executing | 4000 tokens | 0 tokens |

Savings: ~-25% reduction in typical usage

---

*This skill was auto-generated from MCP server configuration*
*Generator: mcp-to-skill (uv-managed)*
