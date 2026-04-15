# Magic Docs Prompt

**Source:** `src/services/MagicDocs/prompts.ts`  
**Type:** Service Prompt  
**Function/Variable:** `buildMagicDocsUpdatePrompt()`

## Description

Prompt for auto-updating Magic Doc files based on conversation content. Keeps documentation current (not a changelog).

## Prompt Content

```
IMPORTANT: This message and these instructions are NOT part of the actual user conversation...

Based on the user conversation above, update the Magic Doc file to incorporate any NEW learnings...

The file {{docPath}} has already been read for you. Here are its current contents:
<current_doc_content>
{{docContents}}
</current_doc_content>

Document title: {{docTitle}}
{{customInstructions}}

CRITICAL RULES FOR EDITING:
- Preserve the Magic Doc header exactly as-is: # MAGIC DOC: {{docTitle}}
- Keep the document CURRENT — this is NOT a changelog
- Update information IN-PLACE — do NOT append historical notes
- Remove or replace outdated information
- BE TERSE. High signal only.

DOCUMENTATION PHILOSOPHY:
- Documentation is for OVERVIEWS, ARCHITECTURE, and ENTRY POINTS
- Do NOT duplicate information obvious from reading the source code
- Focus on: WHY things exist, HOW components connect, WHERE to start reading

What TO document: high-level architecture, non-obvious patterns, key entry points, important design decisions
What NOT to document: obvious code, exhaustive API docs, step-by-step implementation details
```

## Notes

- Custom prompt can be placed at `~/.claude/magic-docs/prompt.md`
- Variables `{{docContents}}`, `{{docPath}}`, `{{docTitle}}`, `{{customInstructions}}` are substituted at runtime
