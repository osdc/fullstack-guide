---
course: python
slug: setup
title: Python Setup
description: "Install Python, set up your development environment, and run your first Python program."
---

This guide helps you install Python, run a first program, create a virtual environment, and install packages.

You can write Python in VS Code, Notepad, or any other text editor. The editor does not run Python. The terminal runs Python.

> [!NOTE]
> You do not need a special Python editor. If you can save a plain-text `.py` file, you can run it from a terminal.

# Windows

## 1. Install Python from the Microsoft Store (recommended)

For most beginners, the Microsoft Store is the simplest way to install Python:

1. Open the **Microsoft Store**.
2. Search for **Python 3**.
3. Choose a current Python 3 release from the **Python Software Foundation**.
4. Select **Get** or **Install**.

Close and reopen any terminal that was already open after installation.

> [!NOTE]
> The Microsoft Store may be blocked on lab PCs. If it does not open, does not show Python, or installation is blocked, use the python.org method below instead.

## 2. Install Python from python.org (fallback)

If the Microsoft Store method is unavailable or causes problems, download Python 3 from:

```text
https://www.python.org/downloads/
```

Open the installer. On the first screen, tick:

```text
Add python.exe to PATH
```

Then select **Install Now**.

> [!IMPORTANT]
> After installing Python or changing PATH, close and reopen the terminal before checking `python --version`.

## 3. Check Python

Close any terminal that was already open. Open a new PowerShell or Command Prompt window and run:

```powershell
python --version
```

You should see something similar to:

```text
Python 3.13.7
```

Also check pip:

```powershell
python -m pip --version
```

If both commands work, Python is ready and you can continue to [run your first program](#7-run-a-first-program).

If you see a message like:

```text
'python' is not recognized as the name of a command
```

then Python is either not installed correctly or was not added to PATH. Try these steps in order:

1. Close and reopen the terminal.
2. If you installed Python from the Microsoft Store and `python` still does not work, check that the Store installation completed and reopen the terminal.
3. If you installed Python from python.org, run the installer again and make sure **Add python.exe to PATH** is enabled.
4. If necessary, add the python.org installation to PATH using the steps below.

> [!NOTE]
> The `py` launcher is optional. Microsoft Store installations may provide `python` without providing `py`.

## 4. Turn Off the Microsoft Store Alias

Sometimes Windows opens the Microsoft Store when you type `python` instead of running an installed Python version. This happens because Windows has a placeholder alias named `python.exe`.

> [!NOTE]
> If you installed Python from the Microsoft Store and `python --version` already works, leave these aliases enabled. Turn them off only when they open the Store unexpectedly or conflict with another Python installation.

Turn it off:

1. Open **Windows Settings**.
2. Open **Apps**.
3. Select **Advanced app settings**.
4. Select **App execution aliases**.
5. Turn off `python.exe` and `python3.exe` if they point to the Microsoft Store.
6. Close and reopen the terminal.
7. Run `python --version` again.

Searching Windows Settings for **App execution aliases** is usually the quickest way to find this page.

## 5. Add Python to PATH Manually

Use this only if running the python.org installer again with **Add python.exe to PATH** does not fix the problem. If you installed Python from the Microsoft Store and `python --version` works, Python is already available and you do not need this section.

Find the installation without using a Python command: open the Start menu, search for **Python**, right-click the Python result, and choose **Open file location**. If this opens a shortcut, right-click the shortcut and choose **Open file location** again. The folder containing `python.exe` is the folder you need.

For a python.org installation, its `Scripts` folder is usually next to the folder containing `python.exe`. Add both folders to PATH. If you cannot find Python, run the python.org installer again and select **Add python.exe to PATH**.

To edit PATH:

1. Search Windows for `environment variables`.
2. Open **Edit the system environment variables**.
3. Select **Environment Variables**.
4. Under **User variables**, select `Path` and choose **Edit**.
5. Add both folders with **New**.
6. Confirm all dialogs.
7. Open a new terminal and run `python --version` again.

> [!WARNING]
> Do not delete the existing PATH entries. Deleting existing PATH entries can break Windows commands and other installed programs. Add the Python folders as new entries instead.

## 6. Allow Virtual-Environment Activation in PowerShell

When you later activate a virtual environment, PowerShell may say that running scripts is disabled.

For the current Windows user, run PowerShell normally and enter:

```powershell
Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy Unrestricted
```

Confirm the prompt. This does not require administrator access.

`Unrestricted` allows more scripts to run. Use this command only on a computer where you understand the setting. Do not run unknown scripts.

> [!WARNING]
> This changes PowerShell's script restrictions for your current Windows user. Do not run scripts you do not trust.

## 7. Run a First Program

Open Notepad and save this as `hello.py`:

```python
print("Hello from OSDC")
```

When saving in Notepad:

- Set **Save as type** to **All files**.
- Save the file as `hello.py`, not `hello.py.txt`.

Open a terminal in the folder containing the file. In File Explorer, open the folder, click the address bar, type `powershell`, and press Enter.

Run the program:

```powershell
python hello.py
```

# macOS

## 1. Install Python

Download Python 3 from:

```text
https://www.python.org/downloads/macos/
```

Open the installer and follow the steps. It provides the `python3` command.

If Homebrew is already installed, you can use:

```bash
brew install python
```

## 2. Check Python

Open the **Terminal** application and run:

```bash
python3 --version
python3 -m pip --version
```

You should see a Python 3 version. On macOS, use `python3` rather than `python` because `python` may not exist or may refer to another system tool.

If `python3` is not found, reopen Terminal. If it still does not work, reinstall Python from the official installer or check your shell PATH.

## 3. Run a First Program

Create a file named `hello.py` in any editor:

```python
print("Hello from OSDC")
```

Open Terminal in that file's folder and run:

```bash
python3 hello.py
```

# Linux

## 1. Install Python

Most Linux distros (distributions) already come with Python installed by default. First check whether Python 3 is available:

```bash
python3 --version
```

If that command works, you may only need to install the package tools required for this workshop. If it does not work, install Python using your distribution's package manager below.

Use the package manager for your Linux distribution.

Ubuntu or Debian:

```bash
sudo apt update
sudo apt install python3 python3-pip python3-venv
```

Fedora:

```bash
sudo dnf install python3 python3-pip
```

Arch Linux:

```bash
sudo pacman -S python python-pip
```

## 2. Check Python

On Ubuntu, Debian, and Fedora, run:

```bash
python3 --version
python3 -m pip --version
```

On Arch Linux, either `python` or `python3` may be available:

```bash
python --version
python -m pip --version
```

Linux package installations normally add Python to PATH automatically. If the command is not found, use your distro's package manager documentation and open a new terminal after installation.

## 3. Run a First Program

Create a file named `hello.py` in any editor:

```python
print("Hello from OSDC")
```

Open a terminal in that file's folder and run:

```bash
python3 hello.py
```

# Using Any Editor

VS Code, Notepad, Notepad++, nvim, vim, nano, emacs and other editors can be used to write Python files.

Code::Blocks is not recommended for Python because it is primarily designed for C and C++, but it can still be used as an editor.

The basic workflow is always:

1. Create a plain-text file ending in `.py`.
2. Write Python code.
3. Save the file.
4. Open a terminal in the file's folder.
5. Run the file with Python.

## VS Code

1. Install VS Code from:

   ```text
   https://code.visualstudio.com/
   ```

2. Install the official **Python** extension by Microsoft.
3. Open your project folder.
4. Open a `.py` file.
5. Select the Python interpreter if VS Code asks.
6. Open the integrated terminal with **Terminal > New Terminal**.
7. Run the file from the terminal:

   ```powershell
   python filename.py
   ```

On macOS and Linux, use `python3 filename.py` before activating a virtual environment.

VS Code also provides a **Run Python File** button in the top-right of a Python file. It uses the interpreter selected in VS Code.

## Notepad

Notepad is enough for the first Python programs. Save the file with the `.py` extension and make sure it does not become `.py.txt`.

# Virtual Environments

A virtual environment is a separate Python setup for one project. Packages installed inside it stay separate from other projects.

> [!TIP]
> Create and activate one virtual environment per project. Install workshop packages only after the environment is active.

Create the environment inside your project folder.

## Windows PowerShell

```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
```

## Windows Command Prompt

```cmd
python -m venv .venv
.venv\Scripts\activate.bat
```

## macOS and Linux

```bash
python3 -m venv .venv
source .venv/bin/activate
```

After activation, the terminal usually begins with `(.venv)`. Check that the environment is active:

```bash
python --version
python -m pip --version
```

On Windows, use the same commands after activation.

# Installing Packages

Activate the virtual environment first, then install the package with Python's pip module.

```bash
python -m pip install requests
```

For the FastAPI workshop:

```bash
python -m pip install "fastapi[standard]"
```

Install packages listed in an existing project file:

```bash
python -m pip install -r requirements.txt
```

Using `python -m pip` makes sure the package is installed for the Python interpreter currently being used.

# Deactivating the Environment

When you are finished working, run:

```bash
deactivate
```

You can activate the same `.venv` again the next time you work on the project.

# Troubleshooting

## `python` Is Not Recognized on Windows

If you installed Python from python.org, you can optionally check whether its launcher is available:

```powershell
py --version
```

The `py` command may not exist, especially when Python was installed from the Microsoft Store. If `py` is also not recognized, reinstall Python from python.org with **Add python.exe to PATH** enabled. You can then run programs with `python filename.py`.

## Python Opens the Microsoft Store

Turn off the `python.exe` and `python3.exe` entries in Windows **App execution aliases**, then reopen the terminal.

## PowerShell Does Not Allow Activation

Run this command as your normal user:

```powershell
Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy Unrestricted
```

Then try activation again.

## The File Is Called `.py.txt`

Enable **File name extensions** in File Explorer and rename the file so it ends exactly in `.py`.

## A Package Cannot Be Imported

Make sure the virtual environment is active, then install the package again:

```bash
python -m pip install package-name
```

