# Team Create Tool Prompt

**Source:** `src/tools/TeamCreateTool/prompt.ts`  
**Type:** Tool Prompt  
**Function/Variable:** `getPrompt()`

## Description

Creates a multi-agent team with a shared task list. Teams coordinate multiple agents working in parallel on complex tasks.

## Prompt Content

```
# TeamCreate

## When to Use

Use this tool proactively whenever:
- The user explicitly asks to use a team, swarm, or group of agents
- The user mentions wanting agents to work together, coordinate, or collaborate
- A task is complex enough that it would benefit from parallel work by multiple agents

When in doubt about whether a task warrants a team, prefer spawning a team.

## Choosing Agent Types for Teammates

When spawning teammates via the Agent tool, choose the `subagent_type` based on what tools the agent needs:
- **Read-only agents** (e.g., Explore, Plan) cannot edit or write files.
- **Full-capability agents** (e.g., general-purpose) have access to all tools including file editing.
- **Custom agents** defined in `.claude/agents/` may have their own tool restrictions.

```
{
  "team_name": "my-project",
  "description": "Working on feature X"
}
```

This creates:
- A team file at `~/.claude/teams/{team-name}/config.json`
- A corresponding task list directory at `~/.claude/tasks/{team-name}/`

## Team Workflow

1. **Create a team** with TeamCreate
2. **Create tasks** using the Task tools (TaskCreate, TaskList, etc.)
3. **Spawn teammates** using the Agent tool with `team_name` and `name` parameters
4. **Assign tasks** using TaskUpdate with `owner` to give tasks to idle teammates
5. **Teammates work on assigned tasks** and mark them completed via TaskUpdate
6. **Teammates go idle between turns** - be patient with idle teammates!
7. **Shutdown your team** - when the task is completed, gracefully shut down teammates via SendMessage with `message: {type: "shutdown_request"}`.

## Automatic Message Delivery

Messages from teammates are automatically delivered to you — you do NOT need to manually check your inbox.

Teammates go idle after every turn — this is completely normal and expected. Idle simply means they are waiting for input.

## Task List Coordination

Teammates should:
1. Check TaskList after completing each task to find available work
2. Claim unassigned, unblocked tasks with TaskUpdate (set `owner` to their name)
3. **Prefer tasks in ID order** (lowest ID first) when multiple tasks are available

**IMPORTANT notes for communication**:
- Always refer to teammates by their NAME
- Do NOT use terminal tools to view team activity; always send a message to teammates via SendMessage
- Do NOT send structured JSON status messages
- Use TaskUpdate to mark tasks completed
```

## Notes

- Tool name: `TeamCreate` (from `src/tools/TeamCreateTool/constants.ts`)
- `DESCRIPTION = 'Create a new multi-agent team'`
- Team config stored at `~/.claude/teams/{team-name}/config.json`
- Task list stored at `~/.claude/tasks/{team-name}/`
- Teams have 1:1 correspondence with task lists
