# Python-venv Launcher Scripts

Generic, drop-in launcher scripts (`run.ps1` / `run.sh`) that automatically create a Python virtual environment, install dependencies, and launch your app — with robust broken-venv detection and smart Python discovery. Copy one into any Python project, edit three lines, and you're done.

---

## Quick Start

1. Copy `run.ps1` (Windows) or `run.sh` (Linux/macOS/Git Bash) into your project root.
2. Edit the `# --- CONFIGURATION ---` block at the top of the script.
3. Run it:

```powershell
# Windows PowerShell
.\run.ps1

# Linux / macOS / Git Bash
bash run.sh
```

The script will:
- Detect and recreate a broken venv automatically
- Create the venv on first run using the best available Python
- Install dependencies from `requirements.txt` (only when the file changes)
- Launch your entry point, forwarding any extra arguments

---

## Configuration Reference

Edit these variables at the top of the script:

| Variable | PowerShell | Bash | Default | Description |
|---|---|---|---|---|
| App display name | `$AppName` | `APP_NAME` | `"My App"` | Shown in status messages |
| Entry point | `$EntryPoint` | `ENTRY_POINT` | `"app.py"` | Main Python script to run |
| Venv directory | `$VenvDir` | `VENV_DIR` | `"venv"` | Where the venv is created |
| Requirements file | `$Requirements` | `REQUIREMENTS` | `"requirements.txt"` | Pip dependencies file |

---

## Platform Support

| Platform | Script | Notes |
|---|---|---|
| Windows (PowerShell) | `run.ps1` | Uses `py` launcher, falls back to common install paths |
| Linux | `run.sh` | Tries `python3`, `python`, then `/usr/bin/python3`, `/usr/local/bin/python3` |
| macOS | `run.sh` | Same as Linux; also checks `~/.pyenv/shims/python3` |
| Git Bash (Windows) | `run.sh` | Activates via `Scripts/activate` when `bin/activate` is absent |

---

## Requirements

- **Python 3.x** installed on the host machine (system-level, not in a venv)
- On Debian/Ubuntu, `python3-venv` must be installed:
  ```
  sudo apt install python3-venv
  ```

---

## How It Works

### Broken venv detection
Both scripts read `pyvenv.cfg` directly (never invoking the potentially broken venv Python) to check whether the base Python interpreter still exists on disk. If not, the venv is wiped and recreated.

### Smart Python discovery (PowerShell)
Priority order: `py` launcher → common install directories (`%LOCALAPPDATA%\Programs\Python\...`, `C:\Python*\`) → PATH.

### Smart Python discovery (Bash)
Priority order: `python3` → `python` → `/usr/bin/python3` → `/usr/local/bin/python3` → `~/.pyenv/shims/python3`.

### Dependency install marker
A hidden `.deps_installed` file is written inside the venv after a successful `pip install`. On subsequent runs the marker's timestamp is compared to `requirements.txt` — dependencies are only reinstalled when the file is newer.

---

## License

MIT
