# Suggested Improvements to Python-venv

*Generated 2026-02-18*

---

## Bug Fixes

### 1. Missing `requirements.txt` is not guarded in either script

In `run.ps1` line 110, `(Get-Item $Requirements).LastWriteTime` throws `ItemNotFoundException` if the file does not exist. In `run.sh` lines 93–97, `pip install -r "$REQUIREMENTS"` fails similarly. A new project with no deps file will crash. Both scripts should skip the install block if the requirements file is absent.

### 2. `run.sh` should use `exec` to launch Python

`run.sh` ends with:
```bash
python "$ENTRY_POINT" "$@"
```
The launcher shell process stays alive as a wrapper around Python. Replacing it with:
```bash
exec python "$ENTRY_POINT" "$@"
```
replaces the shell process with Python, which is the correct pattern for a launcher script — cleaner process tree and proper signal handling (SIGTERM, SIGINT, etc.).

---

## High-Impact Features

### 3. Minimum Python version enforcement

Add an optional `MIN_PYTHON` config variable (e.g. `"3.10"`). After `find_python` locates an interpreter, verify it meets the requirement before creating the venv. Without this check the scripts might silently pick Python 3.7 and the app fails later with cryptic import errors.

### 4. Command-line `--reset` flag

Allow users to force venv recreation without editing the script:
```bash
bash run.sh --reset
```
Currently the only way to force a clean rebuild is to delete the `venv/` directory manually. A flag is more discoverable and safer.

### 5. `uv` support as an optional fast path

[`uv`](https://github.com/astral-sh/uv) is now the dominant tool for venv creation and pip installs — typically 10–100x faster than pip. Both scripts could check for `uv` first and use `uv venv` / `uv pip install` when available, falling back to standard pip/venv. This is a significant usability win for anyone already using `uv`.

---

## Medium-Impact Improvements

### 6. Remove `PLAN_Python-Venv-project.md` from the public repo

`PLAN_Python-Venv-project.md` is an internal planning document. It adds noise for people who clone the repo to use the scripts. It should either be moved out of version control or added to `.gitignore`.

### 7. Address PowerShell execution policy in README

The most common friction point for `run.ps1` is:
```
.\run.ps1 : File cannot be loaded because running scripts is disabled on this system.
```
The README does not mention `Set-ExecutionPolicy` or the `Unblock-File` workaround. A note should be added to the Quick Start section, for example:
```powershell
# One-time fix — run PowerShell as Administrator
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

### 8. Link `Using_Python_Virtual_Environments.md` from README

This file is in the repo but the README never references it. Either add a "Further reading" link at the bottom of the README or remove the file if it is considered redundant with the README content.

---

## Low-Impact / Nice to Have

### 9. `--verbose` flag

Both scripts pass `-q` to `pip install`, which silences all output. This makes debugging dependency failures difficult. A `--verbose` flag (or `VERBOSE=1 bash run.sh` convention) could drop the `-q` flag and surface pip output when needed.

### 10. Example project

A small `example/` directory containing a minimal `hello.py` and `requirements.txt` would make the repo instantly usable for testing without setting up a real project first. It also serves as a live demonstration that the scripts work as documented.

---

## Priority Summary

| # | Item | Priority |
|---|------|----------|
| 1 | Guard against missing `requirements.txt` | Bug — fix first |
| 2 | Use `exec` in `run.sh` | Bug — fix first |
| 3 | Minimum Python version enforcement | High |
| 4 | `--reset` flag | High |
| 5 | `uv` support | High |
| 6 | Remove plan doc from repo | Medium |
| 7 | Execution policy note in README | Medium |
| 8 | Link reference guide from README | Medium |
| 9 | `--verbose` flag | Low |
| 10 | Example project | Low |
