# File Write Tool Prompt

**Source:** `src/tools/FileWriteTool/prompt.ts`  
**Type:** Tool Prompt  
**Function/Variable:** `getWriteToolDescription()`

## Description

Writes (creates or overwrites) files on the local filesystem. Must read existing files first before overwriting them.

## Prompt Content

```
Writes a file to the local filesystem.

Usage:
- This tool will overwrite the existing file if there is one at the provided path.
- If this is an existing file, you MUST use the Read tool first to read the file's contents. This tool will fail if you did not read the file first.
- Prefer the Edit tool for modifying existing files — it only sends the diff. Only use this tool to create new files or for complete rewrites.
- NEVER create documentation files (*.md) or README files unless explicitly requested by the User.
- Only use emojis if the user explicitly requests it. Avoid writing emojis to files unless asked.
```

## Notes

- Tool name exported as `FILE_WRITE_TOOL_NAME = 'Write'`
- The pre-read instruction is conditional on whether the file already exists
- The tool description is built by `getWriteToolDescription()` which embeds `FILE_READ_TOOL_NAME` to avoid hardcoding
