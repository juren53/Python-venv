# Changelog

All notable changes to this project will be documented here.
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

---

## [v0.0.1] — 2026-02-18

### Added
- `run.ps1` — PowerShell launcher for Windows with:
  - Broken venv detection via `pyvenv.cfg` inspection
  - Smart Python discovery (`py` launcher → common install paths → PATH)
  - Marker-based dependency install (reinstalls only when `requirements.txt` changes)
  - Parameterized `# --- CONFIGURATION ---` block
- `run.sh` — Bash launcher for Linux/macOS/Git Bash at parity with `run.ps1`:
  - `test_venv_valid()` — reads `pyvenv.cfg`, checks base Python still exists
  - `find_python()` — tries `python3`, `python`, then common fixed paths
  - Broken venv wipe-and-recreate logic
  - Marker-based dependency install
  - Parameterized `# --- CONFIGURATION ---` block
- `README.md` — usage guide, configuration reference, platform support matrix
- `Using_Python_Virtual_Environments.md` — reference guide for Python venvs
- `.gitignore` — standard Python ignores

[v0.0.1]: https://github.com/juren53/Python-venv/releases/tag/v0.0.1
