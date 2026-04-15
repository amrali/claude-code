# Team Delete Tool Prompt

**Source:** `src/tools/TeamDeleteTool/prompt.ts`  
**Type:** Tool Prompt  
**Function/Variable:** `getPrompt()`

## Description

Removes a team and its associated task directory when swarm work is complete. Fails if team still has active members.

## Prompt Content

```
# TeamDelete

Remove team and task directories when the swarm work is complete.

This operation:
- Removes the team directory (`~/.claude/teams/{team-name}/`)
- Removes the task directory (`~/.claude/tasks/{team-name}/`)
- Clears team context from the current session

**IMPORTANT**: TeamDelete will fail if the team still has active members. Gracefully terminate teammates first, then call TeamDelete after all teammates have shut down.

Use this when all teammates have finished their work and you want to clean up the team resources. The team name is automatically determined from the current session's team context.
```

## Notes

- Tool name: `TeamDelete` (from `src/tools/TeamDeleteTool/constants.ts`)
- Must shut down all teammates via `SendMessage` with `{type: "shutdown_request"}` before calling this tool
