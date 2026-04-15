# Prompt Catalogue

A structured catalogue of all prompts used in the Claude Code repository.

## Directory Structure

```
prompt-catalogue/
├── system-prompts/       - Main system prompt and security instructions
├── agent-prompts/        - Built-in agent definitions and system prompts
├── tool-prompts/         - Per-tool description/prompt strings
│   ├── file-operations/  - File read, write, edit, notebook
│   ├── search/           - Glob, Grep, LSP, ToolSearch
│   ├── web/              - WebFetch, WebSearch
│   ├── shell/            - Bash, PowerShell
│   ├── agent-tasks/      - Agent, Task CRUD, SendMessage
│   ├── mcp/              - MCP tools
│   ├── planning/         - Plan mode entry/exit
│   ├── worktree/         - Worktree entry/exit
│   ├── scheduling/       - CronSchedule
│   ├── user-interaction/ - AskUserQuestion, Sleep
│   ├── config/           - Config tool
│   ├── team/             - Team create/delete
│   ├── todos/            - TodoWrite
│   ├── skills/           - Skill tool
│   ├── remote/           - RemoteTrigger
│   └── mode/             - Brief/SendUserMessage
├── service-prompts/      - Internal service prompts (compact, memory, docs)
├── utility-prompts/      - Utility prompts (buddy, chrome)
└── build-tasks/          - Verbatim copies of prompts/*.md build task files
```

## Source

All prompts are extracted from TypeScript source files under `src/`. Build tasks are copied verbatim from `prompts/*.md`.
