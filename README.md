# academic-code-release

A [Cursor Agent Skill](https://docs.cursor.com/agent/skills) that turns a messy research codebase into a clean, submission-quality code supplement and publishes it to GitHub — in one conversation.

## What it does

When you tell your Cursor agent things like:

> "帮我整理代码准备投稿发布" / "prepare code supplement for my paper" / "发布到 GitHub"

the skill guides the agent through a 5-phase workflow:

| Phase | What happens |
|-------|-------------|
| **1. Code cleanup** | Translate Chinese → English, remove dead code, debug prints, `tqdm`, version annotations |
| **2. Paper consistency** | Verify every formula and parameter in the code matches the paper; annotate discrepancies with `# NOTE:` |
| **3. Readiness audit** | Automated scan for CJK characters, unused imports, and broken module imports |
| **4. Documentation** | Write `README.md` (with figure/table reproduction guide), `requirements.txt`, `.gitignore` |
| **5. GitHub publish** | `git init` → first commit → `git remote add` → `git push` |

The final output is a repository where any reader can clone, `pip install -r requirements.txt`, and reproduce every figure and table in the paper.

## Installation

Copy `SKILL.md` into your Cursor skills directory:

```bash
# Personal (available in all your projects)
cp SKILL.md ~/.cursor/skills/academic-code-release/SKILL.md

# Or clone the whole repo
git clone https://github.com/ChaoanLi/academic-code-release \
    ~/.cursor/skills/academic-code-release
```

Cursor picks up skills automatically — no restart needed.

## Trigger phrases

The skill activates when you use phrases like:

- `帮我整理代码发布` / `准备发布代码` / `投稿代码`
- `prepare code supplement` / `publication-ready repo`
- `发布到 GitHub` / `push to GitHub`

## Designed for

- Python research codebases (NumPy / SciPy / Matplotlib stack)
- Papers with simulation experiments and pre-computed results
- Repos that started as internal scripts (Chinese comments, debug prints, version history in comments)
- Windows (PowerShell) — heredoc workarounds included

## Contents

```
academic-code-release/
└── SKILL.md    # Full 5-phase workflow with audit scripts and checklists
```

## License

MIT
