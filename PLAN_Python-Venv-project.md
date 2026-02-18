# Plan: Python-venv Project Setup

## Context
The csv-reader project produced battle-tested `run.ps1` and `run.sh` launcher scripts through
real-world debugging. This project extracts them into a standalone, reusable repo so the scripts
can be dropped into any Python app with minimal configuration.

---

## Goals
1. Make both scripts **generic parameterized templates** (config vars at the top)
2. Bring **run.sh up to parity** with run.ps1 (broken venv detection, smarter Python finder)
3. Initialize a **public GitHub repo** at github.com/juren53/Python-venv
4. Add a **README.md** explaining usage
5. Add **CHANGELOG.md** with version numbering starting at v0.0.1

---

## Step 1 — Update run.ps1 (generic template)

File: `run.ps1`

Changes:
- Replace "CSVreader launcher" header with "Python venv launcher — generic template"
- Add a clearly marked `# --- CONFIGURATION ---` block at the top with:
  - `$AppName`      — display name, e.g. "My App"
  - `$EntryPoint`   — e.g. "app.py"
  - `$VenvDir`      — default "venv"
  - `$Requirements` — default "requirements.txt"
- Keep all robust logic unchanged: `Test-VenvValid`, `Find-Python`, marker-based dep install

---

## Step 2 — Update run.sh (parity with run.ps1)

File: `run.sh`

Changes:
- Replace "CSVreader launcher" header with generic header
- Add `# --- CONFIGURATION ---` block at top (same variables as ps1, bash style)
- Add `test_venv_valid()` function — reads `pyvenv.cfg`, extracts `home =` line, checks
  if `$home/python3` or `$home/python` exists
- Add `find_python()` function — tries in order:
  1. `python3` (standard Linux/macOS)
  2. `python`
  3. Common paths: `/usr/bin/python3`, `/usr/local/bin/python3`, `~/.pyenv/shims/python3`
- Wipe and recreate venv if `test_venv_valid` fails (same logic as ps1)
- Keep marker-based dependency install

---

## Step 3 — Initialize git repo and push to GitHub

```
cd C:\Users\juren\Projects\Python-venv
git init
git add .
git commit -m "Initial commit: generic Python venv launcher scripts"
gh repo create Python-venv --public --source=. --remote=origin --push
```

---

## Step 4 — Add README.md

Content:
- What this project is (2-sentence intro)
- Quick start: copy `run.ps1` or `run.sh` into your project, edit the CONFIGURATION block
- Configuration reference table
- Platform support matrix (Windows PS / Linux / macOS / Git Bash)
- How it works (broken venv detection, Python discovery, marker-based deps)
- Requirements: Python 3.x installed on the host

---

## Step 5 — Add .gitignore

Standard Python .gitignore — exclude `venv/`, `__pycache__/`, `*.pyc`, `.deps_installed`

---

## Step 6 — Add CHANGELOG.md and tag v0.0.1

- Create `CHANGELOG.md` following Keep a Changelog format
- Tag the repo at `v0.0.1`
- Push tag to GitHub
- Create a formal GitHub release for v0.0.1

---

## Files

| File | Action |
|------|--------|
| `run.ps1` | Modified — generalized header + config block |
| `run.sh` | Modified — generalized + parity features added |
| `README.md` | Created |
| `.gitignore` | Created |
| `CHANGELOG.md` | Created |
| `Using_Python_Virtual_Environments.md` | Added to repo as-is |

---

## Status: Complete — v0.0.1 released 2026-02-18
