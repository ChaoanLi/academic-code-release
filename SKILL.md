---
name: academic-code-release
description: Clean up and publish a Python research code repository as a paper supplement. Covers translating non-English comments to English, removing dead code and debug prints, checking paper-code consistency, running a publication-readiness audit, writing README/requirements, initializing git, and pushing to a new GitHub repository. Use when preparing code to accompany an academic paper submission, or when the user says "帮我整理代码发布", "准备发布代码", "code supplement", "投稿代码", "发布到 GitHub", "publication-ready repo".
---

# Academic Code Release

Workflow for turning a messy research codebase into a clean, submission-quality supplement and publishing it to GitHub.

## Phase 1 — Code Cleanup

Apply to every `.py` file:

### Language
- Translate all non-English comments, docstrings, variable names, and print strings to English. Zero non-ASCII characters in code (except string literals that are user-facing output).

### Dead code
- Remove unused imports, commented-out code blocks, and deprecated compatibility shims.
- Remove version annotations (e.g., v1/v2 tags, changelog comments, "core changes" blocks).

### Diagnostic output
- Remove all `print()` debug statements and `tqdm` progress bars from library/utility code.
- Keep minimal progress prints only in the top-level runner script (e.g., `run_experiments.py`, `main.py`).

### Docstrings
- Rewrite to be concise and academic. Use paper notation where applicable.
- One-line summary for simple functions; params/returns only when non-obvious.
- Remove derivation notes that duplicate content already in the paper.

### Config / data files
- Remove deprecated fields and unused config classes.

## Phase 2 — Paper Consistency Check

Read the paper and verify each key formula and parameter in the code matches. Annotate any discrepancy with `# NOTE:`. Typical things to check:
- Objective / loss functions
- Key mathematical quantities (representers, kernels, variance formulas)
- Experiment parameters (sample sizes, noise levels, replications, kernel hyperparameters)
- Any approximations or numerical substitutions

## Phase 3 — Publication Readiness Audit

Run these checks before release. Adapt the module list to the actual project structure.

```python
import os, re, sys

# 1. Non-ASCII characters in source files — zero tolerance
non_ascii = re.compile(r'[^\x00-\x7F]')
for root, dirs, files in os.walk('.'):
    dirs[:] = [d for d in dirs if d not in ['__pycache__', 'results', '.git']]
    for f in files:
        if f.endswith('.py') or f == 'README.md':
            path = os.path.join(root, f)
            for i, line in enumerate(open(path, encoding='utf-8'), 1):
                if non_ascii.search(line):
                    print(f'{path}:{i}: {line.rstrip()}')

# 2. tqdm usage
for root, dirs, files in os.walk('.'):
    dirs[:] = [d for d in dirs if d not in ['__pycache__', 'results', '.git']]
    for f in files:
        if f.endswith('.py'):
            path = os.path.join(root, f)
            for i, line in enumerate(open(path, encoding='utf-8'), 1):
                if 'tqdm' in line:
                    print(f'{path}:{i}: {line.rstrip()}')

# 3. Import test — enumerate all project modules and verify they load
# Replace the list below with the actual modules in the project.
MODULES = [
    # 'package.module',
    # 'package.subpackage.module',
]
sys.path.insert(0, '.')
for m in MODULES:
    try:
        __import__(m)
        print(f'OK  {m}')
    except Exception as e:
        print(f'ERR {m}: {e}')
```

Fix all flagged issues before proceeding.

## Phase 4 — README & requirements.txt

**README.md** sections (in order):
1. One-paragraph description — paper title, venue, arXiv/DOI if available
2. Requirements — Python version, key packages
3. Installation — `pip install -r requirements.txt`
4. Usage — how to run all experiments
5. Reproducing paper figures and tables — for **every** figure and table in the PDF, list: PDF number, page, output file, and a code snippet to extract the values
6. Repository structure — one-line description per file/directory
7. Citation — BibTeX block

**requirements.txt**: list only packages actually imported. Remove packages that were removed during cleanup.

**`.gitignore`** essentials:
```
__pycache__/
*.py[cod]
.env
*.egg-info/
dist/
build/
.DS_Store
```
Do **not** ignore `results/` if pre-computed outputs should be committed alongside the code.

## Phase 5 — Git Init & GitHub Push

### Initialize repo (run inside the project directory)

```bash
git init
git add .
git status --short   # verify the file list looks correct
git commit -m "Initial release: code supplement for <Paper Title>"
```

> **Windows / PowerShell note**: heredoc (`<<'EOF'`) is not supported. Use a single-line `-m` string or write the message to a temp file.

### Create GitHub repo
Go to https://github.com/new:
- Name: descriptive repo name (e.g., `my-paper-code`)
- Description: one-line paper description
- Visibility: **Public**
- **Do NOT** initialize with README / .gitignore / license (we provide our own)

### Push

```bash
git remote add origin https://github.com/<username>/<repo-name>.git
git branch -M main
git push -u origin main
```

## Checklist

- [ ] Zero non-English characters in all source files and `README.md`
- [ ] No version annotations or inline changelog comments
- [ ] No `tqdm`, no debug `print()` in library/utility code
- [ ] No unused imports
- [ ] All modules import cleanly
- [ ] Paper consistency verified; discrepancies annotated with `# NOTE:`
- [ ] `results/` contains all expected output files (CSVs, figures)
- [ ] `requirements.txt` matches actual imports
- [ ] `README.md` maps every paper figure/table to its output file
- [ ] `.gitignore` in place
- [ ] First commit made and pushed to GitHub
