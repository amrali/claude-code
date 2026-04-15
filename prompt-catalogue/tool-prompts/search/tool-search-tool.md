# Tool Search Tool Prompt

**Source:** `src/tools/ToolSearchTool/prompt.ts`  
**Type:** Tool Prompt  
**Function/Variable:** `getPrompt()`

## Description

Fetches full schema definitions for deferred tools so they can be called. Deferred tools are announced by name but their full parameter schemas are not included in the initial prompt — this tool loads them on demand.

## Prompt Content

```
Fetches full schema definitions for deferred tools so they can be called.

Deferred tools appear by name in <system-reminder> messages. Until fetched, only the name is known — there is no parameter schema, so the tool cannot be invoked. This tool takes a query, matches it against the deferred tool list, and returns the matched tools' complete JSONSchema definitions inside a <functions> block. Once a tool's schema appears in that result, it is callable exactly like any tool defined at the top of the prompt.

Result format: each matched tool appears as one <function>{"description": "...", "name": "...", "parameters": {...}}</function> line inside the <functions> block — the same encoding as the tool list at the top of this prompt.

Query forms:
- "select:Read,Edit,Grep" — fetch these exact tools by name
- "notebook jupyter" — keyword search, up to max_results best matches
- "+slack send" — require "slack" in the name, rank by remaining terms
```

## Notes

- Tool name: `ToolSearch` (from `src/tools/ToolSearchTool/constants.ts`)
- `getToolLocationHint()` varies by feature flag: either "Deferred tools appear in `<system-reminder>` messages" (delta enabled) or "Deferred tools appear in `<available-deferred-tools>` messages"
- `isDeferredTool(tool)`: logic determining which tools are deferred — MCP tools are always deferred; `ToolSearch` itself is never deferred; `Agent` is not deferred when fork subagent is enabled
- The `Brief`/`SendUserMessage` tool is never deferred when KAIROS is active
