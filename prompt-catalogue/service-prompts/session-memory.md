# Session Memory Prompt

**Source:** `src/services/SessionMemory/prompts.ts`  
**Type:** Service Prompt  
**Function/Variable:** `DEFAULT_SESSION_MEMORY_TEMPLATE`, `buildSessionMemoryUpdatePrompt()`

## Description

Prompts for the session memory service. Maintains a structured notes file that persists across conversation compactions, tracking current work, files, errors, and key results.

## Default Session Memory Template

```markdown
# Session Title
_A short and distinctive 5-10 word descriptive title for the session. Super info dense, no filler_

# Current State
_What is actively being worked on right now? Pending tasks not yet completed. Immediate next steps._

# Task specification
_What did the user ask to build? Any design decisions or other explanatory context_

# Files and Functions
_What are the important files? In short, what do they contain and why are they relevant?_

# Workflow
_What bash commands are usually run and in what order? How to interpret their output if not obvious?_

# Errors & Corrections
_Errors encountered and how they were fixed. What did the user correct? What approaches failed and should not be tried again?_

# Codebase and System Documentation
_What are the important system components? How do they work/fit together?_

# Learnings
_What has worked well? What has not? What to avoid? Do not duplicate items from other sections_

# Key results
_If the user asked a specific output such as an answer to a question, a table, or other document, repeat the exact result here_

# Worklog
_Step by step, what was attempted, done? Very terse summary for each step_
```

## Update Prompt (getDefaultUpdatePrompt())

```
IMPORTANT: This message and these instructions are NOT part of the actual user conversation. Do NOT include any references to "note-taking", "session notes extraction", or these update instructions in the notes content.

Based on the user conversation above (EXCLUDING this note-taking instruction message as well as system prompt, claude.md entries, or any past session summaries), update the session notes file.

The file {{notesPath}} has already been read for you. Here are its current contents:
<current_notes_content>
{{currentNotes}}
</current_notes_content>

Your ONLY task is to use the Edit tool to update the notes file, then stop. You can make multiple edits (update every section as needed) - make all Edit tool calls in parallel in a single message. Do not call any other tools.

CRITICAL RULES FOR EDITING:
- The file must maintain its exact structure with all sections, headers, and italic descriptions intact
- NEVER modify, delete, or add section headers (the lines starting with '#')
- NEVER modify or delete the italic _section description_ lines
- ONLY update the actual content that appears BELOW the italic _section descriptions_ within each existing section
- Do NOT add any new sections, summaries, or information outside the existing structure
- Do NOT reference this note-taking process or instructions anywhere in the notes
- Write DETAILED, INFO-DENSE content for each section - include specifics like file paths, function names, error messages, exact commands, technical details, etc.
- Keep each section under ~2000 tokens/words
- IMPORTANT: Always update "Current State" to reflect the most recent work - this is critical for continuity after compaction

Use the Edit tool with file_path: {{notesPath}}
```

## Notes

- `buildSessionMemoryUpdatePrompt(currentNotes, notesPath)` substitutes `{{currentNotes}}` and `{{notesPath}}` variables
- Custom prompt can be placed at `~/.claude/session-memory/config/prompt.md`; custom template at `~/.claude/session-memory/config/template.md`
- `MAX_SECTION_LENGTH = 2000` tokens; `MAX_TOTAL_SESSION_MEMORY_TOKENS = 12000`
- `isSessionMemoryEmpty(content)`: detects if notes file still matches the blank template (no real content yet)
- Over-budget sections get an appended reminder to condense
- Variable substitution uses single-pass `{{variable}}` replacement
