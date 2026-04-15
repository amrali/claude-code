# Config Tool Prompt

**Source:** `src/tools/ConfigTool/prompt.ts`  
**Type:** Tool Prompt  
**Function/Variable:** `generatePrompt()`

## Description

Gets or sets Claude Code configuration settings. Dynamically generates a list of all configurable settings from the `SUPPORTED_SETTINGS` registry.

## Prompt Content

```
Get or set Claude Code configuration settings.

View or change Claude Code settings. Use when the user requests configuration changes, asks about current settings, or when adjusting a setting would benefit them.

## Usage
- **Get current value:** Omit the "value" parameter
- **Set new value:** Include the "value" parameter

## Configurable settings list
The following settings are available for you to change:

### Global Settings (stored in ~/.claude.json)
[Dynamically generated list of global settings with types/options and descriptions]

### Project Settings (stored in settings.json)
[Dynamically generated list of project settings with types/options and descriptions]

## Model
- model - Override the default model. Available options:
  - "sonnet": [description]
  - "opus": [description]
  - "haiku": [description]
  - (other available models)

## Examples
- Get theme: { "setting": "theme" }
- Set dark theme: { "setting": "theme", "value": "dark" }
- Enable vim mode: { "setting": "editorMode", "value": "vim" }
- Enable verbose: { "setting": "verbose", "value": true }
- Change model: { "setting": "model", "value": "opus" }
- Change permission mode: { "setting": "permissions.defaultMode", "value": "plan" }
```

## Notes

- Tool name: `Config` (from `src/tools/ConfigTool/constants.ts`)
- `DESCRIPTION = 'Get or set Claude Code configuration settings.'`
- `generatePrompt()` builds the settings list dynamically from `SUPPORTED_SETTINGS` registry at runtime
- Voice settings (`voiceEnabled`) are hidden when GrowthBook kills the voice feature
- Model section dynamically fetches available model options from `getModelOptions()`
