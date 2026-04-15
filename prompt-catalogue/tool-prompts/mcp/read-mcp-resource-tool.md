# Read MCP Resource Tool Prompt

**Source:** `src/tools/ReadMcpResourceTool/prompt.ts`  
**Type:** Tool Prompt  
**Function/Variable:** `DESCRIPTION`, `PROMPT`

## Description

Reads a specific resource from an MCP server by server name and resource URI.

## Prompt Content

```
Reads a specific resource from an MCP server, identified by server name and resource URI.

Parameters:
- server (required): The name of the MCP server from which to read the resource
- uri (required): The URI of the resource to read
```

## Notes

- Tool name inferred from file location: `ReadMcpResource`
- Also exports a shorter `DESCRIPTION` constant used for the tool schema description
