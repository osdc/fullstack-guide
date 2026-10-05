---
course: python
slug: file-handling
title: File Handling
description: "Learn how to create, read, write, append, and manage files using Python."
---

File handling allows a Python program to store data permanently and read data created by other programs.

Variables store data temporarily in memory. Files store data on a storage device, so the data can still be available after the program exits.

Common file-handling tasks include:

- Creating files
- Opening files
- Reading files
- Writing files
- Appending data
- Renaming and deleting files
- Working with directories
- Reading and writing JSON and CSV data
- Handling file-related errors

# File Paths

A file path tells Python where a file is located.

A relative path starts from the current working directory:

```text
data/workshops.txt
```

An absolute path describes the complete location of a file:

```text
C:\projects\osdc\data\workshops.txt
```

Use `pathlib.Path` for platform-independent paths instead of manually joining strings.

```python
from pathlib import Path

file_path = Path("data") / "workshops.txt"
print(file_path)
```

The `/` operator joins path parts correctly on Windows, macOS, and Linux.

## The Current Working Directory

The current working directory is the folder from which a Python program is run.

```python
from pathlib import Path

current_directory = Path.cwd()
print(current_directory)
```

A relative path is interpreted from this directory. The current working directory may be different from the directory containing the Python file, so use clear project paths when possible.

# Opening a File

Use the built-in `open()` function to open a file.

```python
file = open("workshops.txt", "r", encoding="utf-8")

# Work with the file here.

file.close()
```

The arguments are:

1. The file path
2. The mode
3. The text encoding

Always specify `encoding="utf-8"` when working with text files unless another encoding is required.

Closing a file releases operating-system resources and ensures pending data is written.

# The `with` Statement

The preferred way to open a file is with a `with` statement. Python closes the file automatically when the block ends, even if an error occurs.

```python
with open("workshops.txt", "r", encoding="utf-8") as file:
    contents = file.read()

print(contents)
```

The variable `file` is available inside the `with` block. The file is closed automatically after the block.

This is safer than manually calling `close()`:

```python
with open("workshops.txt", encoding="utf-8") as file:
    print(file.closed)  # False inside the block

print(file.closed)      # True after the block
```

# File Modes

The mode controls how a file is opened.

| Mode | Purpose |
|---|---|
| `r` | Read an existing file |
| `w` | Write to a file, replacing existing content |
| `a` | Append to the end of a file |
| `x` | Create a new file, failing if it already exists |
| `b` | Binary mode, combined with another mode |
| `t` | Text mode, the default |
| `+` | Enable both reading and writing |

Examples:

```python
open("workshops.txt", "r", encoding="utf-8")   # read
open("workshops.txt", "w", encoding="utf-8")   # write
open("workshops.txt", "a", encoding="utf-8")   # append
open("image.png", "rb")                         # read binary data
open("image.png", "wb")                         # write binary data
```

The default mode is `r`, and the default type is text mode.

# Writing to a Text File

Use `w` mode to write text to a file.

```python
with open("club.txt", "w", encoding="utf-8") as file:
    file.write("OSDC\n")
    file.write("JIIT, Noida\n")
    file.write("Open Source Development\n")
```

If the file does not exist, Python creates it. If it already exists, `w` mode replaces its content.

## Writing Multiple Lines

Use `writelines()` to write multiple strings. It does not add new lines automatically.

```python
workshops = [
    "HTML and CSS\n",
    "JavaScript\n",
    "Python and FastAPI\n"
]

with open("workshops.txt", "w", encoding="utf-8") as file:
    file.writelines(workshops)
```

You can also join the lines before writing:

```python
workshops = ["HTML and CSS", "JavaScript", "Python and FastAPI"]

with open("workshops.txt", "w", encoding="utf-8") as file:
    file.write("\n".join(workshops))
```

# Appending to a File

Use `a` mode to add content at the end without replacing existing content.

```python
with open("attendance.txt", "a", encoding="utf-8") as file:
    file.write("Python Workshop: 45\n")
```

Appending is useful for logs and records that should grow over time.

```python
from datetime import datetime

with open("osdc.log", "a", encoding="utf-8") as file:
    timestamp = datetime.now().isoformat()
    file.write(f"{timestamp} - Workshop viewed\n")
```

# Creating a New File with `x`

Use `x` mode when the program should create a file only if it does not already exist.

```python
try:
    with open("new_workshop.txt", "x", encoding="utf-8") as file:
        file.write("Python and FastAPI Workshop")
except FileExistsError:
    print("The file already exists.")
```

This prevents accidentally replacing an existing file.

# Reading a Text File

## Reading the Complete File

Use `.read()` to read the complete file as one string.

```python
with open("club.txt", "r", encoding="utf-8") as file:
    contents = file.read()

print(contents)
```

You can provide a number to read only a certain number of characters:

```python
with open("club.txt", encoding="utf-8") as file:
    first_five_characters = file.read(5)

print(first_five_characters)
```

## Reading One Line

Use `.readline()` to read one line at a time.

```python
with open("workshops.txt", encoding="utf-8") as file:
    first_line = file.readline()
    second_line = file.readline()

print(first_line.strip())
print(second_line.strip())
```

A line usually includes its ending newline character. Use `.strip()` when that newline should be removed.

## Reading All Lines

Use `.readlines()` to return a list of lines.

```python
with open("workshops.txt", encoding="utf-8") as file:
    lines = file.readlines()

print(lines)
```

The resulting list may contain newline characters:

```python
[
    "HTML and CSS\n",
    "JavaScript\n",
    "Python and FastAPI\n"
]
```

## Iterating Through a File

Iterating over the file is memory-efficient because Python processes one line at a time.

```python
with open("workshops.txt", encoding="utf-8") as file:
    for line in file:
        print(line.strip())
```

This is preferred for large files instead of reading the entire file at once.

# File Position

A file object keeps track of its current position.

```python
with open("club.txt", encoding="utf-8") as file:
    print(file.tell())
    print(file.read(4))
    print(file.tell())
```

Use `.seek()` to move to a specific position.

```python
with open("club.txt", encoding="utf-8") as file:
    file.read(4)
    file.seek(0)
    contents = file.read()

print(contents)
```

`seek(0)` moves the position back to the beginning.

# Reading and Writing with `r+`

The `r+` mode allows reading and writing an existing file. It does not create a missing file.

```python
with open("club.txt", "r+", encoding="utf-8") as file:
    contents = file.read()
    file.write("\nUpdated by OSDC.")
```

Be careful when mixing reading and writing because the current file position controls where new data is written.

# Handling File Errors

Trying to open a missing file in read mode raises `FileNotFoundError`.

```python
try:
    with open("missing.txt", encoding="utf-8") as file:
        contents = file.read()
except FileNotFoundError:
    print("The file was not found.")
```

Other common exceptions include:

- `FileNotFoundError`: the path does not exist
- `FileExistsError`: a file already exists when using `x` mode
- `PermissionError`: the program does not have permission
- `IsADirectoryError`: a directory was used where a file was expected
- `UnicodeDecodeError`: the file encoding does not match the requested encoding

Handle only the errors that you expect and can respond to.

```python
from pathlib import Path

file_path = Path("workshops.txt")

if not file_path.exists():
    print("The workshop file does not exist.")
else:
    print(file_path.read_text(encoding="utf-8"))
```

# Using `pathlib`

`pathlib` provides an object-oriented interface for filesystem paths.

```python
from pathlib import Path

file_path = Path("data") / "workshops.txt"

print(file_path)
print(file_path.name)
print(file_path.stem)
print(file_path.suffix)
print(file_path.parent)
```

For `data/workshops.txt`:

- `.name` is `workshops.txt`
- `.stem` is `workshops`
- `.suffix` is `.txt`
- `.parent` is `data`

## Reading and Writing with `pathlib`

```python
from pathlib import Path

file_path = Path("club.txt")
file_path.write_text("OSDC\nJIIT, Noida", encoding="utf-8")

contents = file_path.read_text(encoding="utf-8")
print(contents)
```

These methods are convenient for small text files. For large files, use `open()` and process the file incrementally.

## Checking Paths

```python
from pathlib import Path

file_path = Path("workshops.txt")

print(file_path.exists())
print(file_path.is_file())
print(file_path.is_dir())
```

# Directories

Use `Path.mkdir()` to create a directory.

```python
from pathlib import Path

data_directory = Path("data")
data_directory.mkdir(exist_ok=True)
```

`exist_ok=True` prevents an error if the directory already exists.

Create parent directories with `parents=True`:

```python
from pathlib import Path

logs_directory = Path("project") / "data" / "logs"
logs_directory.mkdir(parents=True, exist_ok=True)
```

List files and directories with `.iterdir()`:

```python
from pathlib import Path

data_directory = Path("data")

for item in data_directory.iterdir():
    print(item)
```

Find files matching a pattern:

```python
from pathlib import Path

data_directory = Path("data")

for file_path in data_directory.glob("*.txt"):
    print(file_path)
```

Use `.rglob()` to search recursively through subdirectories:

```python
for file_path in Path("project").rglob("*.json"):
    print(file_path)
```

# Renaming and Deleting Files

Use `.rename()` to rename or move a file.

```python
from pathlib import Path

old_path = Path("old_workshops.txt")
new_path = Path("workshops.txt")

old_path.rename(new_path)
```

Use `.unlink()` to delete a file.

```python
from pathlib import Path

file_path = Path("temporary.txt")

if file_path.exists():
    file_path.unlink()
```

Check carefully before deleting files. `unlink()` permanently removes the file instead of sending it to a recycle bin.

# JSON Files

JSON is a common format for storing structured data and communicating with web APIs. Python's built-in `json` module can convert between Python objects and JSON.

## Writing JSON

Use `json.dump()` to write Python data to a file.

```python
import json

workshop = {
    "id": 1,
    "title": "Python and FastAPI Workshop",
    "topic": "backend",
    "seats": 40,
    "is_active": True
}

with open("workshop.json", "w", encoding="utf-8") as file:
    json.dump(workshop, file, indent=2)
```

The resulting file is formatted like this:

```json
{
  "id": 1,
  "title": "Python and FastAPI Workshop",
  "topic": "backend",
  "seats": 40,
  "is_active": true
}
```

Python values are converted to JSON values:

| Python | JSON |
|---|---|
| `dict` | Object |
| `list` or `tuple` | Array |
| `str` | String |
| `int` or `float` | Number |
| `True` or `False` | `true` or `false` |
| `None` | `null` |

## Reading JSON

Use `json.load()` to read JSON from a file.

```python
import json

with open("workshop.json", encoding="utf-8") as file:
    workshop = json.load(file)

print(workshop["title"])
print(workshop["seats"])
```

## Handling Invalid JSON

Invalid JSON raises `json.JSONDecodeError`.

```python
import json

try:
    with open("workshop.json", encoding="utf-8") as file:
        workshop = json.load(file)
except FileNotFoundError:
    print("The JSON file was not found.")
except json.JSONDecodeError:
    print("The JSON file is invalid.")
```

## Converting JSON Strings

Use `json.dumps()` and `json.loads()` when working with JSON strings instead of files.

```python
import json

workshop = {
    "name": "OSDC",
    "topic": "FastAPI"
}

json_text = json.dumps(workshop)
print(json_text)

python_data = json.loads(json_text)
print(python_data["topic"])
```

# CSV Files

CSV, or Comma-Separated Values, stores tabular data. It is commonly used for attendance records and spreadsheet exports.

Example `attendance.csv`:

```text
workshop,attendance
Python,45
FastAPI,35
JavaScript,50
```

## Reading CSV with `csv.reader`

```python
import csv

with open("attendance.csv", newline="", encoding="utf-8") as file:
    reader = csv.reader(file)

    for row in reader:
        print(row)
```

The first row is treated as a normal row by `csv.reader`.

## Reading CSV with `csv.DictReader`

`DictReader` uses the first row as column names.

```python
import csv

with open("attendance.csv", newline="", encoding="utf-8") as file:
    reader = csv.DictReader(file)

    for row in reader:
        print(row["workshop"], row["attendance"])
```

Values read from CSV files are strings. Convert numeric fields when necessary:

```python
import csv

with open("attendance.csv", newline="", encoding="utf-8") as file:
    reader = csv.DictReader(file)

    for row in reader:
        attendance = int(row["attendance"])
        print(row["workshop"], attendance)
```

## Writing CSV

Use `csv.DictWriter` to write dictionaries as rows.

```python
import csv

attendance = [
    {"workshop": "Python", "attendance": 45},
    {"workshop": "FastAPI", "attendance": 35}
]

with open("attendance.csv", "w", newline="", encoding="utf-8") as file:
    fieldnames = ["workshop", "attendance"]
    writer = csv.DictWriter(file, fieldnames=fieldnames)

    writer.writeheader()
    writer.writerows(attendance)
```

# Binary Files

Text files store characters. Binary files store raw bytes, such as images, PDFs, audio, and compiled files.

Use `rb` to read binary data and `wb` to write binary data.

```python
with open("source-image.png", "rb") as source:
    image_data = source.read()

with open("copy-image.png", "wb") as destination:
    destination.write(image_data)
```

For large binary files, copy them in chunks instead of reading the entire file into memory:

```python
chunk_size = 1024 * 1024

with open("source-image.png", "rb") as source:
    with open("copy-image.png", "wb") as destination:
        while chunk := source.read(chunk_size):
            destination.write(chunk)
```

The walrus operator `:=` assigns the chunk and checks whether it is non-empty in the same expression.

# Temporary Files

Use the `tempfile` module for temporary data instead of inventing temporary filenames.

```python
from tempfile import TemporaryDirectory
from pathlib import Path

with TemporaryDirectory() as temporary_directory:
    file_path = Path(temporary_directory) / "workshop.txt"
    file_path.write_text("Temporary OSDC data", encoding="utf-8")
    print(file_path.read_text(encoding="utf-8"))
```

The temporary directory and its contents are removed automatically when the block ends.

# File Metadata

`Path.stat()` provides metadata about a file.

```python
from pathlib import Path

file_path = Path("workshops.txt")

if file_path.exists():
    metadata = file_path.stat()
    print("Size:", metadata.st_size, "bytes")
    print("Modified:", metadata.st_mtime)
```

Useful attributes include:

- `st_size`: file size in bytes
- `st_mtime`: last modification time
- `st_ctime`: platform-dependent creation or metadata-change time

# Safe File Handling

Follow these practices when working with files:

- Use `with open(...)` so files close automatically.
- Use `pathlib.Path` for portable paths.
- Specify the correct text encoding.
- Validate user-provided paths.
- Avoid overwriting files accidentally; use `x` mode when appropriate.
- Handle expected exceptions.
- Do not trust filenames or paths received from users.
- Do not expose passwords, tokens, or private files.
- Keep uploaded files outside sensitive application directories.
- Limit upload size in web applications.
- Avoid constructing shell commands directly from file input.

## Avoiding Path Traversal

Never directly combine an untrusted filename with a sensitive directory.

Unsafe pattern:

```python
# Do not use untrusted input this way.
filename = input("Filename: ")
file_path = Path("uploads") / filename
```

A filename such as `../../private.txt` could refer to a file outside the uploads directory.

For user uploads, validate and constrain the path. A basic filename-only approach is:

```python
from pathlib import Path

filename = Path(input("Filename: ")).name
file_path = Path("uploads") / filename
```

Web applications need additional validation, size limits, and safe storage policies.

# File Handling with FastAPI

FastAPI can receive uploaded files using `UploadFile`.

Install the required multipart package if it is not already installed:

```bash
pip install python-multipart
```

Example upload endpoint:

```python
from pathlib import Path

from fastapi import FastAPI, File, UploadFile

app = FastAPI()
UPLOAD_DIRECTORY = Path("uploads")
UPLOAD_DIRECTORY.mkdir(exist_ok=True)


@app.post("/upload")
async def upload_file(file: UploadFile = File(...)):
    destination = UPLOAD_DIRECTORY / Path(file.filename).name

    with destination.open("wb") as output_file:
        while chunk := await file.read(1024 * 1024):
            output_file.write(chunk)

    return {
        "filename": destination.name,
        "content_type": file.content_type
    }
```

In a production application, also validate file types, enforce size limits, generate safe unique names, and store uploads using an appropriate storage service.

# Complete Example: OSDC Workshop Data

This example stores a list of workshops in a JSON file.

```python
import json
from pathlib import Path

DATA_FILE = Path("workshops.json")


def load_workshops():
    if not DATA_FILE.exists():
        return []

    try:
        with DATA_FILE.open(encoding="utf-8") as file:
            return json.load(file)
    except json.JSONDecodeError:
        return []


def save_workshops(workshops):
    with DATA_FILE.open("w", encoding="utf-8") as file:
        json.dump(workshops, file, indent=2)


def add_workshop(title, topic, seats):
    workshops = load_workshops()
    next_id = len(workshops) + 1

    workshops.append({
        "id": next_id,
        "title": title,
        "topic": topic,
        "seats": seats
    })

    save_workshops(workshops)


add_workshop("Python and FastAPI", "backend", 40)
print(load_workshops())
```

The functions separate responsibilities:

- `load_workshops()` reads and parses the file
- `save_workshops()` writes structured data
- `add_workshop()` updates the collection

# Quick Reference

```python
from pathlib import Path

file_path = Path("data") / "workshops.txt"
file_path.parent.mkdir(parents=True, exist_ok=True)

with file_path.open("w", encoding="utf-8") as file:
    file.write("Python and FastAPI\n")

with file_path.open(encoding="utf-8") as file:
    for line in file:
        print(line.strip())
```

| Operation | Example |
|---|---|
| Open text file | `open("file.txt", encoding="utf-8")` |
| Read all text | `file.read()` |
| Read one line | `file.readline()` |
| Read lines | `file.readlines()` |
| Write text | `file.write(text)` |
| Append text | `open("file.txt", "a")` |
| Create only if missing | `open("file.txt", "x")` |
| Check existence | `path.exists()` |
| Check file | `path.is_file()` |
| Create directory | `path.mkdir(parents=True, exist_ok=True)` |
| Find files | `path.glob("*.txt")` |
| Rename or move | `path.rename(new_path)` |
| Delete file | `path.unlink()` |
| Read JSON | `json.load(file)` |
| Write JSON | `json.dump(data, file, indent=2)` |
| Read CSV | `csv.DictReader(file)` |
| Write CSV | `csv.DictWriter(file, fieldnames=...)` |
