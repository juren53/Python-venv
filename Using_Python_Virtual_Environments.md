
# Setting Up and Using Python Virtual Environments

Virtual environments isolate project dependencies so each project has its own set of packages, avoiding conflicts with other projects or the global Python install.

## Creating a Virtual Environment

Navigate to your project folder and create a virtual environment:

```powershell
cd C:\Users\<username>\Projects\myproject
python -m venv .venv
```

This creates a `.venv` folder containing a local copy of Python and pip.

## Activating the Virtual Environment

### PowerShell

```powershell
.\.venv\Scripts\Activate.ps1
```

### Command Prompt

```cmd
.\.venv\Scripts\activate.bat
```

### Git Bash

```bash
source .venv/Scripts/activate
```

When activated, your prompt will show `(.venv)` at the beginning to indicate you're inside the virtual environment.

## Installing Packages

With the virtual environment activated, use pip as usual. Packages install into the `.venv` folder, not globally:

```powershell
pip install requests
pip install flask
```

## Saving and Restoring Dependencies

Save your project's dependencies to a file:

```powershell
pip freeze > requirements.txt
```

Restore them on another machine or fresh environment:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

## Deactivating the Virtual Environment

```powershell
deactivate
```

This returns you to the global Python environment.

## Deleting a Virtual Environment

Simply delete the `.venv` folder:

```powershell
Remove-Item -Recurse -Force .venv
```

## Notes

- Always add `.venv` to your `.gitignore` — virtual environments should not be committed to source control.
- Use `requirements.txt` to share dependencies instead.
- Each project should have its own virtual environment.
- If you get a PowerShell execution policy error when activating, run: `Set-ExecutionPolicy -Scope CurrentUser RemoteSigned`
