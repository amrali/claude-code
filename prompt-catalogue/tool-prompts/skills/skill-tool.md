# Skill Tool Prompt

**Source:** `src/tools/SkillTool/prompt.ts`  
**Type:** Tool Prompt  
**Function/Variable:** `getPrompt()` (async, memoized)

## Description

Executes a named skill (slash command) within the main conversation. Skills provide specialized capabilities and are invoked by name.

## Prompt Content

```
Execute a skill within the main conversation

When users ask you to perform tasks, check if any of the available skills match. Skills provide specialized capabilities and domain knowledge.

When users reference a "slash command" or "/<something>" (e.g., "/commit", "/review-pr"), they are referring to a skill. Use this tool to invoke it.

How to invoke:
- Use this tool with the skill name and optional arguments
- Examples:
  - `skill: "pdf"` - invoke the pdf skill
  - `skill: "commit", args: "-m 'Fix bug'"` - invoke with arguments
  - `skill: "review-pr", args: "123"` - invoke with arguments
  - `skill: "ms-office-suite:pdf"` - invoke using fully qualified name

Important:
- Available skills are listed in system-reminder messages in the conversation
- When a skill matches the user's request, this is a BLOCKING REQUIREMENT: invoke the relevant Skill tool BEFORE generating any other response about the task
- NEVER mention a skill without actually calling this tool
- Do not invoke a skill that is already running
- Do not use this tool for built-in CLI commands (like /help, /clear, etc.)
- If you see a <command-name> tag in the current conversation turn, the skill has ALREADY been loaded - follow the instructions directly instead of calling this tool again
```

## Notes

- Tool name: `Skill` (from `src/tools/SkillTool/constants.ts` as `SKILL_TOOL_NAME`)
- `getPrompt()` is memoized by CWD (since skills are CWD-specific)
- The `<command-name>` tag is the `COMMAND_NAME_TAG` constant from `src/constants/xml.ts`
- Skill listings are budget-controlled: `SKILL_BUDGET_CONTEXT_PERCENT = 0.01` (1% of context window)
- Bundled skills always get full descriptions; non-bundled may be truncated
- `MAX_LISTING_DESC_CHARS = 250` per skill entry
