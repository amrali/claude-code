# Notebook Edit Tool Prompt

**Source:** `src/tools/NotebookEditTool/prompt.ts`  
**Type:** Tool Prompt  
**Function/Variable:** `PROMPT`

## Description

Replaces the contents of a specific cell in a Jupyter notebook. Supports insert, delete, and replace edit modes.

## Prompt Content

```
Completely replaces the contents of a specific cell in a Jupyter notebook (.ipynb file) with new source. Jupyter notebooks are interactive documents that combine code, text, and visualizations, commonly used for data analysis and scientific computing. The notebook_path parameter must be an absolute path, not a relative path. The cell_number is 0-indexed. Use edit_mode=insert to add a new cell at the index specified by cell_number. Use edit_mode=delete to delete the cell at the index specified by cell_number.
```

## Notes

- Tool name: `NotebookEdit` (defined in `src/tools/NotebookEditTool/constants.ts`)
- `DESCRIPTION`: `'Replace the contents of a specific cell in a Jupyter notebook.'`
- Edit modes: `replace` (default), `insert`, `delete`
- `cell_number` is 0-indexed
