# academic-code-release

A [Cursor Agent Skill](https://docs.cursor.com/agent/skills) for organizing a Python research codebase, checking its alignment with a paper, and preparing a documented GitHub code supplement.

## What it does

When you tell your Cursor agent things like:

> "帮我整理代码准备投稿发布" / "prepare code supplement for my paper" / "发布到 GitHub"

the skill guides the agent through a 5-phase workflow:

| Phase | What happens |
|-------|-------------|
| **1. Code cleanup** | Translate Chinese → English, remove dead code, debug prints, `tqdm`, version annotations |
| **2. Paper consistency** | Compare key formulas and parameters with the paper; annotate discrepancies with `# NOTE:` |
| **3. Readiness audit** | Automated scan for CJK characters, unused imports, and broken module imports |
| **4. Documentation** | Write `README.md` (with figure/table reproduction guide), `requirements.txt`, `.gitignore` |
| **5. GitHub publish** | `git init` → first commit → `git remote add` → `git push` |

The goal is a repository with documented installation steps and a traceable
mapping from paper figures and tables to code and outputs. Reproduction depends
on available data, compatible dependencies, compute resources, and successful
execution of each relevant experiment. Code cleanup and import checks alone do
not establish reproducibility.

## Installation

Clone directly into your personal Cursor skills directory:

```bash
mkdir -p ~/.cursor/skills
# Use an empty destination; do not overwrite an existing installation.
git clone https://github.com/ChaoanLi/academic-code-release \
    ~/.cursor/skills/academic-code-release
```

Start a Cursor agent session with skills enabled and request this workflow.

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

## Example workflow

This is a **proposed layout for a simulated project**, not a claim that a
paper's results have been reproduced.

Before:

```text
research/
├── experiment_final_v3.py
├── helpers.py
└── figure.png
```

After organizing and documenting the same work:

```text
research/
├── README.md              # setup, commands, output mapping, and verification status
├── requirements.txt      # dependencies checked in the recorded environment
├── .gitignore
├── run_experiment.py
├── helpers.py
└── results/
    └── figure.png
```

A useful README entry links each output to its evidence:

| Paper item | Command | Output | Verification status |
| --- | --- | --- | --- |
| Figure 1 | `python run_experiment.py --seed 42` | `results/figure.png` | Illustrative command; not executed in this example |

Adapt commands and paths to the real project. Do not rename an entry point
without updating its callers.

## Before publishing

- Record the paper version, code commit, Python/package versions, and data source.
- Run documented commands in a clean environment and inspect the outputs.
- Compare the generated figures and table values with the specified paper version.
- Mark unavailable data, unrun experiments, and numerical mismatches explicitly.
- Review the staged files for credentials, personal records, and data restrictions.
- Check ownership, third-party notices, and license terms before sharing code.
- Confirm the target repository and intended visibility before publishing.

Preserve the scientific method and results during cleanup. Resolve substantive
paper/code discrepancies with the author rather than silently changing the
experiment to match a claim.

## Contents

```
academic-code-release/
├── SKILL.md    # Full 5-phase workflow with audit scripts and checklists
├── README.md   # Installation, scope, example, and release checks
└── LICENSE     # MIT license
```

## License

[MIT](LICENSE).

## Related skills

- [paper-audit](https://github.com/ChaoanLi/paper-audit):
  inspect figure/text consistency, paper/code alignment, and redundant prose in Claude Code.
- [conversational-language-coaching](https://github.com/ChaoanLi/conversational-language-coaching):
  optional language tips during ordinary Claude Code conversations.
