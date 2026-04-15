# Agent Tool Prompt

**Source:** `src/tools/AgentTool/prompt.ts`  
**Type:** Tool Prompt  
**Function/Variable:** `getPrompt(agentDefinitions, isCoordinator?, allowedAgentTypes?)`

## Description

Launches specialized sub-agents (subprocesses) that autonomously handle complex tasks. Supports background execution, parallel launches, worktree isolation, and (when fork subagent is enabled) forking the current agent.

## Prompt Content

```
Launch a new agent to handle complex, multi-step tasks autonomously.

The Agent tool launches specialized agents (subprocesses) that autonomously handle complex tasks. Each agent type has specific capabilities and tools available to it.

Available agent types and the tools they have access to:
[dynamically listed agents]

When using the Agent tool, specify a subagent_type parameter to select which agent type to use. If omitted, the general-purpose agent is used.

When NOT to use the Agent tool:
- If you want to read a specific file path, use the Read tool or the Glob tool instead of the Agent tool, to find the match more quickly
- If you are searching for a specific class definition like "class Foo", use the Glob tool instead, to find the match more quickly
- If you are searching for code within a specific file or set of 2-3 files, use the Read tool instead of the Agent tool, to find the match more quickly
- Other tasks that are not related to the agent descriptions above

Usage notes:
- Always include a short description (3-5 words) summarizing what the agent will do
- Launch multiple agents concurrently whenever possible, to maximize performance
- When the agent is done, it will return a single message back to you. The result returned by the agent is not visible to the user. To show the user the result, you should send a text message back to the user with a concise summary of the result.
- You can optionally run agents in the background using the run_in_background parameter. When an agent runs in the background, you will be automatically notified when it completes — do NOT sleep, poll, or proactively check on its progress.
- **Foreground vs background**: Use foreground (default) when you need the agent's results before you can proceed. Use background when you have genuinely independent work to do in parallel.
- To continue a previously spawned agent, use SendMessage with the agent's ID or name as the `to` field. The agent resumes with its full context preserved. Each Agent invocation starts fresh — provide a complete task description.
- The agent's outputs should generally be trusted
- Clearly tell the agent whether you expect it to write code or just to do research, since it is not aware of the user's intent
- You can optionally set `isolation: "worktree"` to run the agent in a temporary git worktree, giving it an isolated copy of the repository. The worktree is automatically cleaned up if the agent makes no changes.

## Writing the prompt

Brief the agent like a smart colleague who just walked into the room — it hasn't seen this conversation, doesn't know what you've tried, doesn't understand why this task matters.
- Explain what you're trying to accomplish and why.
- Describe what you've already learned or ruled out.
- Give enough context about the surrounding problem that the agent can make judgment calls rather than just following a narrow instruction.
- If you need a short response, say so ("report in under 200 words").
- Lookups: hand over the exact command. Investigations: hand over the question.

Terse command-style prompts produce shallow, generic work.

**Never delegate understanding.** Don't write "based on your findings, fix the bug." Those phrases push synthesis onto the agent. Write prompts that prove you understood: include file paths, line numbers, what specifically to change.
```

## Notes

- Tool name: `Agent` (from `src/tools/AgentTool/constants.ts`)
- `getPrompt()` is async; takes agent definitions and optional flags
- When `isCoordinator=true`, only the shared core section is returned (not the full usage notes)
- When `shouldInjectAgentListInMessages()` is true (GrowthBook flag `tengu_agent_list_attach`), the agent list is sent via an `agent_listing_delta` attachment message instead of being embedded in the tool description
- When fork subagent is enabled: the "When to fork" section and fork examples are included instead of the standard `whenNotToUse` and examples
- Background tasks disabled when `CLAUDE_CODE_DISABLE_BACKGROUND_TASKS=true`
- Remote isolation (`isolation: "remote"`) only for `ant` users
- Concurrency note about launching multiple agents only shown to non-pro subscription users (when list is inline)
