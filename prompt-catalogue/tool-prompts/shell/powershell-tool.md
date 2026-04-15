# PowerShell Tool Prompt

**Source:** `src/tools/PowerShellTool/prompt.ts`  
**Type:** Tool Prompt  
**Function/Variable:** `getPrompt()` (async)

## Description

Executes PowerShell commands on Windows. Provides edition-specific syntax guidance for PowerShell 5.1 vs PowerShell 7+.

## Prompt Content

```
Executes a given PowerShell command with optional timeout. Working directory persists between commands; shell state (variables, functions) does not.

IMPORTANT: This tool is for terminal operations via PowerShell: git, npm, docker, and PS cmdlets. DO NOT use it for file operations (reading, writing, editing, searching, finding files) - use the specialized tools for this instead.

[PowerShell edition: detected at runtime — one of:]
  [Windows PowerShell 5.1]: Pipeline chain operators `&&` and `||` are NOT available. Use `A; if ($?) { B }` instead. Ternary/null-coalescing NOT available. Default encoding UTF-16 LE.
  [PowerShell 7+]: `&&` and `||` ARE available. Ternary, null-coalescing, null-conditional are available. Default encoding UTF-8.
  [Unknown]: Assume Windows PowerShell 5.1 for compatibility.

Before executing the command, please follow these steps:

1. Directory Verification:
   - If the command will create new directories or files, first use `Get-ChildItem` (or `ls`) to verify the parent directory exists and is the correct location

2. Command Execution:
   - Always quote file paths that contain spaces with double quotes
   - Capture the output of the command.

PowerShell Syntax Notes:
   - Variables use $ prefix: $myVar = "value"
   - Escape character is backtick (`), not backslash
   - Use Verb-Noun cmdlet naming: Get-ChildItem, Set-Location, New-Item, Remove-Item
   - Common aliases: ls (Get-ChildItem), cd (Set-Location), cat (Get-Content), rm (Remove-Item)
   - Pipe operator | works similarly to bash but passes objects, not text
   - Registry access uses PSDrive prefixes: `HKLM:\SOFTWARE\...` — NOT raw `HKEY_LOCAL_MACHINE\...`
   - Environment variables: read with `$env:NAME`, set with `$env:NAME = "value"`
   - Call native exe with spaces in path: `& "C:\Program Files\App\app.exe" arg1 arg2`

Interactive and blocking commands (will hang — this tool runs with -NonInteractive):
   - NEVER use `Read-Host`, `Get-Credential`, `Out-GridView`, `$Host.UI.PromptForChoice`, or `pause`
   - Destructive cmdlets may prompt for confirmation. Add `-Confirm:$false` when you intend the action to proceed.
   - Never use `git rebase -i`, `git add -i`, or other commands that open an interactive editor

Passing multiline strings (commit messages) to native executables:
   - Use a single-quoted here-string. The closing `'@` MUST be at column 0:
     git commit -m @'
     Commit message here.
     '@
   - Use @'...'@ (literal) not @"..."@ (interpolated) unless you need variable expansion

Usage notes:
  - The command argument is required.
  - You can specify an optional timeout in milliseconds (up to [max]ms).
  - [Background tasks note if enabled]
  - Avoid using PowerShell to run commands that have dedicated tools:
    - File search: Use Glob (NOT Get-ChildItem -Recurse)
    - Content search: Use Grep (NOT Select-String)
    - Read files: Use Read (NOT Get-Content)
    - Edit files: Use Edit
    - Write files: Use Write (NOT Set-Content/Out-File)
  - When issuing multiple commands:
    - Independent commands: make multiple PowerShell tool calls in a single message
    - Sequential dependent: chain in a single call (see edition-specific syntax)
  - For git commands: prefer new commits over amending; never skip hooks; avoid force push to main
```

## Notes

- Tool name: `PowerShell` (from `src/tools/PowerShellTool/toolName.ts`)
- `getPrompt()` is async — it calls `getPowerShellEdition()` to detect the installed PS version
- Edition detection: `'desktop'` (PowerShell 5.1), `'core'` (PowerShell 7+), or `null` (unknown)
- Background tasks guidance conditional on `CLAUDE_CODE_DISABLE_BACKGROUND_TASKS`
- Sleep guidance conditional on background tasks being enabled
- Timeout values from `getDefaultBashTimeoutMs()` and `getMaxBashTimeoutMs()`
