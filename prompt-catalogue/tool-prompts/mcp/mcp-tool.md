# MCP Tool Prompt

**Source:** `src/tools/MCPTool/prompt.ts`  
**Type:** Tool Prompt  
**Function/Variable:** `PROMPT`, `DESCRIPTION`

## Description

MCP (Model Context Protocol) tool proxy. The actual prompt and description are dynamically populated per-tool by `mcpClient.ts` at connection time based on the tool definitions provided by each MCP server.

## Prompt Content

```
[Empty — overridden dynamically by mcpClient.ts per MCP server tool definition]
```

## Notes

- The file exports `PROMPT = ''` and `DESCRIPTION = ''` as stubs
- Actual descriptions come from the MCP server's `tools/list` response
- Tool names follow the pattern `mcp__<server-name>__<tool-name>`
- MCP tools are always deferred (require `ToolSearch` to load their full schema) unless they set `_meta['anthropic/alwaysLoad']: true`
