# Bash Tool Prompt

**Source:** `src/tools/BashTool/prompt.ts`  
**Type:** Tool Prompt  
**Function/Variable:** `getSimplePrompt()`

## Description

Executes bash commands. Working directory persists between commands; shell state (variables, functions) does not. Includes detailed git/PR instructions, sandbox guidance, and tool preference rules.

## Prompt Content

```
Executes a given bash command and returns its output.

The working directory persists between commands, but shell state does not. The shell environment is initialized from the user's profile (bash or zsh).

IMPORTANT: Avoid using this tool to run `find`, `grep`, `cat`, `head`, `tail`, `sed`, `awk`, or `echo` commands, unless explicitly instructed or after you have verified that a dedicated tool cannot accomplish your task. Instead, use the appropriate dedicated tool as this will provide a much better experience for the user:

 - File search: Use Glob (NOT find or ls)
 - Content search: Use Grep (NOT grep or rg)
 - Read files: Use Read (NOT cat/head/tail)
 - Edit files: Use Edit (NOT sed/awk)
 - Write files: Use Write (NOT echo >/cat <<EOF)
 - Communication: Output text directly (NOT echo/printf)

While the Bash tool can do similar things, it's better to use the built-in tools as they provide a better user experience and make it easier to review tool calls and give permission.

# Instructions
 - If your command will create new directories or files, first use this tool to run `ls` to verify the parent directory exists and is the correct location.
 - Always quote file paths that contain spaces with double quotes in your command
 - Try to maintain your current working directory throughout the session by using absolute paths and avoiding usage of `cd`. You may use `cd` if the User explicitly requests it.
 - You may specify an optional timeout in milliseconds (up to [max]ms). By default, your command will timeout after [default]ms.
 - You can use the `run_in_background` parameter to run the command in the background. Only use this if you don't need the result immediately and are OK being notified when the command completes later.
 - When issuing multiple commands:
   - If the commands are independent and can run in parallel, make multiple Bash tool calls in a single message.
   - If the commands depend on each other and must run sequentially, use a single Bash call with '&&' to chain them together.
   - Use ';' only when you need to run commands sequentially but don't care if earlier commands fail.
   - DO NOT use newlines to separate commands (newlines are ok in quoted strings).
 - For git commands:
   - Prefer to create a new commit rather than amending an existing commit.
   - Before running destructive operations, consider whether there is a safer alternative.
   - Never skip hooks (--no-verify) or bypass signing unless the user has explicitly asked for it.
 - Avoid unnecessary `sleep` commands:
   - Do not sleep between commands that can run immediately — just run them.
   - If your command is long running and you would like to be notified when it finishes — use `run_in_background`. No sleep needed.
   - Do not retry failing commands in a sleep loop — diagnose the root cause.

# Committing changes with git
[Full git commit instructions for external users, including safety protocol, staging, commit message format via HEREDOC, PR creation format via gh CLI]

## Command sandbox
[When sandbox enabled: instructions about filesystem/network restrictions and when to use dangerouslyDisableSandbox]
```

## Notes

- Tool name: `Bash` (from `src/tools/BashTool/toolName.ts`)
- `getSimplePrompt()` is the main exported function
- **Git instructions** (`getCommitAndPRInstructions()`): For `ant` users, a short skills-based section is returned; for external users, full inline git/PR instructions with HEREDOC examples
- **Sandbox section** (`getSimpleSandboxSection()`): Only included when `SandboxManager.isSandboxingEnabled()` returns true
- Background tasks guidance is omitted when `CLAUDE_CODE_DISABLE_BACKGROUND_TASKS=true`
- Ant-native builds omit Glob/Grep tool preference since those tools use embedded bfs/ugrep
- Timeout values come from `getDefaultBashTimeoutMs()` and `getMaxBashTimeoutMs()`
