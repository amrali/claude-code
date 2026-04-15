# Companion Buddy Prompt

**Source:** `src/buddy/prompt.ts`  
**Type:** Utility Prompt  
**Function/Variable:** `companionIntroText()`

## Description

Describes a small companion character (e.g., a cat or other animal) that sits next to the user's input box and occasionally comments. Instructs Claude not to impersonate the companion but to step aside when the user addresses it directly.

## Prompt Content

```
# Companion

A small {species} named {name} sits beside the user's input box and occasionally comments in a speech bubble. You're not {name} — it's a separate watcher.

When the user addresses {name} directly (by name), its bubble will answer. Your job in that moment is to stay out of the way: respond in ONE line or less, or just answer any part of the message meant for you. Don't explain that you're not {name} — they know. Don't narrate what {name} might say — the bubble handles that.
```

## Notes

- Gated by `feature('BUDDY')`
- Injected as a `companion_intro` attachment via `getCompanionIntroAttachment()`
- Only injected once per companion (skips if same companion already announced)
- Suppressed when `getGlobalConfig().companionMuted` is true
