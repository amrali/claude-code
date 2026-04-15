# Ask User Question Tool Prompt

**Source:** `src/tools/AskUserQuestionTool/prompt.ts`  
**Type:** Tool Prompt  
**Function/Variable:** `ASK_USER_QUESTION_TOOL_PROMPT`

## Description

Presents multiple-choice questions to gather user input during execution. Used for clarifying ambiguity, gathering preferences, and offering implementation choices. Supports optional markdown/HTML preview panes for single-select questions.

## Prompt Content

```
Use this tool when you need to ask the user questions during execution. This allows you to:
1. Gather user preferences or requirements
2. Clarify ambiguous instructions
3. Get decisions on implementation choices as you work
4. Offer choices to the user about what direction to take.

Usage notes:
- Users will always be able to select "Other" to provide custom text input
- Use multiSelect: true to allow multiple answers to be selected for a question
- If you recommend a specific option, make that the first option in the list and add "(Recommended)" at the end of the label

Plan mode note: In plan mode, use this tool to clarify requirements or choose between approaches BEFORE finalizing your plan. Do NOT use this tool to ask "Is my plan ready?" or "Should I proceed?" - use ExitPlanMode for plan approval. IMPORTANT: Do not reference "the plan" in your questions (e.g., "Do you have feedback about the plan?", "Does the plan look good?") because the user cannot see the plan in the UI until you call ExitPlanMode. If you need plan approval, use ExitPlanMode instead.
```

## Notes

- Tool name exported as `ASK_USER_QUESTION_TOOL_NAME = 'AskUserQuestion'`
- `DESCRIPTION = 'Asks the user multiple choice questions to gather information, clarify ambiguity, understand preferences, make decisions or offer them choices.'`
- Also exports `PREVIEW_FEATURE_PROMPT` with `markdown` and `html` variants — describes how to use the optional `preview` field on options for side-by-side visual comparison UI
- Preview only supported for single-select questions (not `multiSelect`)
- Chip width: `ASK_USER_QUESTION_TOOL_CHIP_WIDTH = 12`
