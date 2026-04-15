# Glob Tool Prompt

**Source:** `src/tools/GlobTool/prompt.ts`  
**Type:** Tool Prompt  
**Function/Variable:** `DESCRIPTION`

## Description

Fast file pattern matching tool. Returns matching file paths sorted by modification time.

## Prompt Content

```
- Fast file pattern matching tool that works with any codebase size
- Supports glob patterns like "**/*.js" or "src/**/*.ts"
- Returns matching file paths sorted by modification time
- Use this tool when you need to find files by name patterns
- When you are doing an open ended search that may require multiple rounds of globbing and grepping, use the Agent tool instead
```

## Notes

- Tool name exported as `GLOB_TOOL_NAME = 'Glob'`
- The `DESCRIPTION` constant is the entire prompt
