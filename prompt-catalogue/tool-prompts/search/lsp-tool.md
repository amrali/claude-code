# LSP Tool Prompt

**Source:** `src/tools/LSPTool/prompt.ts`  
**Type:** Tool Prompt  
**Function/Variable:** `DESCRIPTION`

## Description

Provides Language Server Protocol code intelligence features including go-to-definition, find-references, hover info, and call hierarchy.

## Prompt Content

```
Interact with Language Server Protocol (LSP) servers to get code intelligence features.

Supported operations:
- goToDefinition: Find where a symbol is defined
- findReferences: Find all references to a symbol
- hover: Get hover information (documentation, type info) for a symbol
- documentSymbol: Get all symbols (functions, classes, variables) in a document
- workspaceSymbol: Search for symbols across the entire workspace
- goToImplementation: Find implementations of an interface or abstract method
- prepareCallHierarchy: Get call hierarchy item at a position (functions/methods)
- incomingCalls: Find all functions/methods that call the function at a position
- outgoingCalls: Find all functions/methods called by the function at a position

All operations require:
- filePath: The file to operate on
- line: The line number (1-based, as shown in editors)
- character: The character offset (1-based, as shown in editors)

Note: LSP servers must be configured for the file type. If no server is available, an error will be returned.
```

## Notes

- Tool name exported as `LSP_TOOL_NAME = 'LSP'`
- Line and character numbers are 1-based (not 0-based)
- Requires LSP servers to be configured for the file type
