# Jupyter Notebook Editor for AI Agents

[![Python 3.6+](https://img.shields.io/badge/python-3.6+-blue.svg)](https://www.python.org/downloads/)
[![Zero Dependencies](https://img.shields.io/badge/dependencies-zero-green.svg)](https://github.com/yourusername/notebook-editor)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A **zero-dependency**, **AI-agent-friendly** command-line tool for programmatically editing Jupyter Notebook (`.ipynb`) files. Designed specifically for Large Language Models (LLMs) and automated workflows.

[Русская версия](README_RU.md) | [English](README.md)

---

## 🎯 Why This Tool?

Traditional Jupyter notebook editing requires either:

- A full Jupyter environment with heavy dependencies
- Manual JSON manipulation (error-prone and fragile)
- Complex libraries that may not work in restricted environments

**This tool solves these problems** by providing:

✅ **Zero Dependencies** - Uses only Python standard library  
✅ **AI-Agent Optimized** - Clear CLI interface with file-based I/O pattern  
✅ **JSON-Safe** - Preserves notebook structure and metadata  
✅ **Portable** - Works anywhere Python 3.6+ is installed  
✅ **Reliable** - Predictable behavior for automated workflows  

---

## 🚀 Quick Start

### Installation

No installation needed! Just download the script:

```bash
wget https://raw.githubusercontent.com/yourusername/notebook-editor/main/notebook_editor.py
chmod +x notebook_editor.py
```

Or clone the repository:

```bash
git clone https://github.com/yourusername/notebook-editor.git
cd notebook-editor
```

### Basic Usage

```bash
# List all cells in a notebook
python3 notebook_editor.py list my_notebook.ipynb

# Read a specific cell with line numbers
python3 notebook_editor.py read my_notebook.ipynb 5 --numbered

# Update a cell from file
python3 notebook_editor.py update my_notebook.ipynb 5 --from-file modified_content.py

# Edit specific lines only (more efficient!)
python3 notebook_editor.py patch my_notebook.ipynb 5 --lines 10-15 --from-file patch.py

# Clear all cell outputs
python3 notebook_editor.py clear-output my_notebook.ipynb --all

# Search for content
python3 notebook_editor.py search my_notebook.ipynb "import pandas"
```

---

## 📖 Complete Command Reference

### 1. **list** - View Notebook Structure

Shows all cells with their indices, types, and content preview.

```bash
python3 notebook_editor.py list <notebook.ipynb> [--limit N] [--json]
```

**Examples:**

```bash
# Standard output
python3 notebook_editor.py list analysis.ipynb --limit 20

# JSON output (for LLM parsing)
python3 notebook_editor.py list analysis.ipynb --json
```

**JSON output:**

```json
{
  "notebook": "analysis.ipynb",
  "total_cells": 15,
  "cells": [
    {"index": 0, "type": "code", "lines": 10, "has_output": true, ...}
  ]
}
```

---

### 2. **read** - Extract Cell Content

Read a specific cell's content to console or file.

```bash
python3 notebook_editor.py read <notebook.ipynb> <index> [--to-file <file>] [--numbered] [--include-output]
```

**Examples:**

```bash
# Print with line numbers (useful for patch command)
python3 notebook_editor.py read analysis.ipynb 5 --numbered

# Save to file
python3 notebook_editor.py read analysis.ipynb 5 --to-file cell_5.py

# Include execution outputs
python3 notebook_editor.py read analysis.ipynb 5 --include-output
```

**Output with --numbered:**

```
--- Cell 5 (code) [17 lines] ---
 1: import pandas as pd
 2: import numpy as np
 3: 
 4: def calculate_mean(data):
 5:     """Calculate the mean."""
 6:     return sum(data) / len(data)
...
```

---

### 3. **search** - Find Content

Search for text or regex patterns across all cells.

```bash
python3 notebook_editor.py search <notebook.ipynb> "<query>" [--regex]
```

**Examples:**

```bash
# Simple text search
python3 notebook_editor.py search analysis.ipynb "import pandas"

# Regex search
python3 notebook_editor.py search analysis.ipynb "def .*_handler" --regex
```

---

### 4. **update** - Modify Cell Content

Replace the entire content of a cell.

```bash
python3 notebook_editor.py update <notebook.ipynb> <index> --from-file <file>
python3 notebook_editor.py update <notebook.ipynb> <index> --content "<text>"
```

**Examples:**

```bash
# Update from file (RECOMMENDED)
python3 notebook_editor.py update analysis.ipynb 5 --from-file modified_code.py

# Keep outputs (don't clear)
python3 notebook_editor.py update analysis.ipynb 5 --from-file code.py --no-clear-output
```

---

### 5. **patch** - Edit Specific Lines

Replace only specified lines in a cell. **Much more efficient than update!**

```bash
python3 notebook_editor.py patch <notebook.ipynb> <index> --lines <range> --from-file <file>
python3 notebook_editor.py patch <notebook.ipynb> <index> --lines <range> --content "<text>"
```

**Examples:**

```bash
# Replace lines 5-10
python3 notebook_editor.py patch analysis.ipynb 3 --lines 5-10 --from-file patch.py

# Replace a single line
python3 notebook_editor.py patch analysis.ipynb 3 --lines 7-7 --content "new_value = 42"

# Insert after line 5 (add code without replacing)
python3 notebook_editor.py patch analysis.ipynb 3 --lines 5 --insert --from-file insert.py

# Disable automatic indent preservation
python3 notebook_editor.py patch analysis.ipynb 3 --lines 5-10 --from-file patch.py --no-preserve-indent
```

**Key features:**
- ✅ Automatically preserves relative indentation
- ✅ Insert mode (`--insert`) — adds code without replacing
- ✅ Auto-clears outputs after edit

---

### 6. **add** - Insert New Cell

Add a new code or markdown cell.

```bash
python3 notebook_editor.py add <notebook.ipynb> --type <code|markdown> --from-file <file>
```

**Examples:**

```bash
# Add at the beginning
python3 notebook_editor.py add analysis.ipynb --index 0 --type markdown --content "# Introduction"

# Add at the end (default)
python3 notebook_editor.py add analysis.ipynb --type code --from-file new_analysis.py
```

---

### 7. **delete** - Remove Cell

```bash
python3 notebook_editor.py delete <notebook.ipynb> <index>
```

---

### 8. **diff** - Preview Changes

Show what will change before updating a cell.

```bash
python3 notebook_editor.py diff <notebook.ipynb> <index> --from-file <file>
```

---

### 9. **create** - New Notebook

```bash
python3 notebook_editor.py create <notebook.ipynb>
```

---

### 10. **clear-output** - Clear Cell Outputs

Remove execution outputs from cells.

```bash
python3 notebook_editor.py clear-output <notebook.ipynb> --all
python3 notebook_editor.py clear-output <notebook.ipynb> --cells 0 2 5
```

**Examples:**

```bash
# Clear all code cells
python3 notebook_editor.py clear-output analysis.ipynb --all

# Clear specific cells
python3 notebook_editor.py clear-output analysis.ipynb --cells 0 2 5
```

---

### 11. **info** - Notebook Metadata

Show notebook information and statistics.

```bash
python3 notebook_editor.py info <notebook.ipynb>
```

**Output:**

```
Notebook: analysis.ipynb
Format: nbformat 4.5
Kernel: Python 3
Cells: 25 total
  - Code: 18
  - Markdown: 7
  - With outputs: 12
Total source lines: 450
```

---

### 12. **validate** - Check Structure

Validate notebook JSON structure.

```bash
python3 notebook_editor.py validate <notebook.ipynb>
```

Returns exit code 1 on errors — useful for CI/CD.

---

### 13. **save-output** - Extract Images

```bash
python3 notebook_editor.py save-output <notebook.ipynb> <index> --to-file <path>
```

---

### 14. **export** - Export to Markdown or Python
322: 
323: Exports the entire notebook to a single Markdown (`.md`) or Python (`.py`) file, including all cell outputs and images.
324: 
325: - For `.py` files: Markdown cells and outputs are saved as `#` comments.
326: - For `.md` files: Markdown cells and outputs are saved as `<!-- -->` comments.
327: - **Images**: Automatically extracted and saved to a folder. If the folder exists, a `_copyN` suffix is added.
328: 
329: ```bash
330: python3 notebook_editor.py export <notebook.ipynb> <output.md|.py> [--image-dir <dir>]
331: ```
332: 
333: **Examples:**
334: 
335: ```bash
336: # Export to python (runnable script with comments)
337: python3 notebook_editor.py export analysis.ipynb script.py
338: 
339: # Export to markdown
340: python3 notebook_editor.py export analysis.ipynb report.md
341: ```
338: 
339: ---
340: 
341: ## 📋 Command Summary Table

| Command | Description | Key Flags |
|---------|-------------|-----------|
| `list` | View structure | `--limit`, `--json` |
| `read` | Read cell | `--numbered`, `--to-file`, `--include-output` |
| `search` | Find text | `--regex` |
| `update` | Replace entire cell | `--from-file`, `--no-clear-output` |
| `patch` | Edit specific lines | `--lines`, `--insert`, `--no-preserve-indent` |
| `add` | Add cell | `--index`, `--type`, `--from-file` |
| `delete` | Remove cell | - |
| `diff` | Preview changes | `--from-file` |
| `clear-output` | Clear outputs | `--all`, `--cells` |
| `info` | Show metadata | - |
| `validate` | Check structure | - |
| `create` | New notebook | - |
| `save-output` | Extract images | `--output-index`, `--to-file` |
| `export` | Export to Markdown | `--image-dir` |

---

## 🤖 Best Practice Workflow for AI Agents

> **Note for Users:** There is a dedicated guide for AI agents located at [`README_AGENT.md`](README_AGENT.md).

### Recommended Workflow

```bash
# 1. Explore: Understand the structure
python3 notebook_editor.py list notebook.ipynb

# 2. Read with line numbers
python3 notebook_editor.py read notebook.ipynb 5 --numbered

# 3. Precise editing (more efficient than update!)
python3 notebook_editor.py patch notebook.ipynb 5 --lines 10-15 --from-file patch.py

# OR full cell replacement
python3 notebook_editor.py read notebook.ipynb 5 --to-file temp.py
# (edit temp.py)
python3 notebook_editor.py update notebook.ipynb 5 --from-file temp.py
```

### Advantages of patch over update

| update | patch |
|--------|-------|
| Must copy entire cell | Edit only needed lines |
| Easy to break indentation | Indentation preserved automatically |
| Uses more tokens | Saves tokens |

---

## 📋 Requirements

- **Python 3.6+** (no external packages required)
- Works on Linux, macOS, and Windows

---

## 🔧 Technical Details

### File Format

The tool works with standard Jupyter Notebook format (`.ipynb`), which is JSON-based:

- Preserves all metadata
- Maintains cell execution counts
- Handles both code and markdown cells
- Supports multi-line content with proper formatting

### Safety Features

- Validates JSON structure before saving
- Creates backups implicitly (use version control!)
- Handles edge cases (empty cells, special characters, etc.)
- Provides clear error messages

---

## 📝 Examples

### Example 1: Quick Fix for a Function

```bash
# View cell with line numbers
python3 notebook_editor.py read notebook.ipynb 3 --numbered

# Replace only lines 5-8
python3 notebook_editor.py patch notebook.ipynb 3 --lines 5-8 --content "    return x * 2"
```

### Example 2: Add Validation to a Function

```bash
# Insert new code after line 4
echo "    if x is None:
        raise ValueError('x cannot be None')" > insert.py

python3 notebook_editor.py patch notebook.ipynb 3 --lines 4 --insert --from-file insert.py
```

### Example 3: Pre-commit Cleanup

```bash
# Clear all outputs
python3 notebook_editor.py clear-output notebook.ipynb --all

# Validate structure
python3 notebook_editor.py validate notebook.ipynb
```

---

## 📄 License

MIT License - feel free to use in your projects!

---

**Made with ❤️ for AI Agents and Developers**
