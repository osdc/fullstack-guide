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

## Contents

- [Windows](#windows)
- [macOS](#macos)
- [Using Any Editor](#using-any-editor)
- [Virtual Environments](#virtual-environments)
- [Installing Packages](#installing-packages)
- [Troubleshooting](#troubleshooting)

# Windows
Follow this article by GeeksForGeeks: 

### https://www.geeksforgeeks.org/python/how-to-install-python-on-windows/

## Run a First Program

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

If this command doesn't work, you might've forgotten to add Python to PATH. So, try the following steps.

## Add Python to PATH Manually

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


# macOS
Follow this article by GeeksForGeeks: 

### https://www.geeksforgeeks.org/python/how-to-install-python-on-mac


## Run a First Program

Create a file named `hello.py` in any editor:

```python
print("Hello from OSDC")
```

Open Terminal in that file's folder and run:

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


