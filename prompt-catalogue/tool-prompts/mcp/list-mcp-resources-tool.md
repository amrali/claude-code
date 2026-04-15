# List MCP Resources Tool Prompt

**Source:** `src/tools/ListMcpResourcesTool/prompt.ts`  
**Type:** Tool Prompt  
**Function/Variable:** `DESCRIPTION`, `PROMPT`

## Description

Lists all available resources from configured MCP servers.

## Prompt Content

```
List available resources from configured MCP servers.
Each returned resource will include all standard MCP resource fields plus a 'server' field 
indicating which server the resource belongs to.

Parameters:
- server (optional): The name of a specific MCP server to get resources from. If not provided,
  resources from all servers will be returned.
```

## Notes

- Tool name: `ListMcpResourcesTool` (exported as `LIST_MCP_RESOURCES_TOOL_NAME`)
- Also exports a shorter `DESCRIPTION` constant used for the tool schema description
