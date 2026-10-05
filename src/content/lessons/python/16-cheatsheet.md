---
course: python
slug: cheatsheet
title: Python Cheatsheet
description: "A quick reference covering Python syntax, data types, conditions, loops, functions, files, OOP, APIs, and common operations."
---

A quick reference for the Python, API, FastAPI, and frontend concepts covered in this workshop.

Use the linked modules for complete explanations and runnable examples.

# Python Basics

## Running Python

```bash
python filename.py
python -m venv .venv
```

Activate a virtual environment on Windows PowerShell:

```powershell
.venv\Scripts\Activate.ps1
```

Activate it on macOS or Linux:

```bash
source .venv/bin/activate
```

## Comments and Indentation

```python
# This is a single-line comment.

if registration_open:
    print("Registrations are open.")
```

Python uses indentation to define code blocks. Use four spaces consistently.

## Variables and Types

```python
club_name = "OSDC"       # str
member_count = 50        # int
event_rating = 4.8       # float
registration_open = True  # bool
nothing = None            # NoneType
```

```python
print(type(member_count))
print(isinstance(member_count, int))
```

Variable names may contain letters, numbers, and underscores, but cannot start with a number or be a Python keyword.

# Operators

## Arithmetic

| Operator | Meaning | Example |
|---|---|---|
| `+` | Addition | `a + b` |
| `-` | Subtraction | `a - b` |
| `*` | Multiplication | `a * b` |
| `/` | Division | `a / b` |
| `//` | Floor division | `a // b` |
| `%` | Remainder | `a % b` |
| `**` | Power | `a ** b` |

```python
first = 17
second = 5

print(first + second)
print(first / second)
print(first // second)
print(first % second)
print(first ** 2)
```

## Comparison

```python
member_count == 50  # equal
member_count != 50  # not equal
member_count > 10   # greater than
member_count < 100  # less than
member_count >= 50  # greater than or equal
member_count <= 50  # less than or equal
```

## Logical Operators

```python
if registration_open and member_count > 0:
    print("Registration is available.")

if technology == "html" or technology == "css":
    print("Frontend technology")

if not registration_open:
    print("Registration is closed.")
```

## Assignment Operators

```python
count = 10
count += 1
count -= 1
count *= 2
count /= 2
```

# Strings

```python
club_name = "OSDC"

print(club_name[0])       # first character
print(club_name[-1])      # last character
print(club_name[0:2])     # slice
print(len(club_name))
print(club_name.lower())
print(club_name.upper())
print(club_name.strip())
print(club_name.replace("OS", "XX"))
```

Strings are immutable. String methods return new strings.

## String Formatting

```python
workshop = "Python and FastAPI"
seats = 40

print(f"{workshop} has {seats} seats.")
print("{} has {} seats.".format(workshop, seats))
```

## Splitting and Joining

```python
text = "HTML,CSS,JavaScript,Python"
technologies = text.split(",")
print(technologies)

formatted = " - ".join(technologies)
print(formatted)
```

# Input and Conversion

```python
club_name = input("Club name: ").strip()
member_count = int(input("Member count: "))
event_rating = float(input("Event rating: "))
```

`input()` always returns a string. Convert it before performing numeric operations.

```python
try:
    member_count = int(input("Member count: "))
except ValueError:
    print("Please enter a whole number.")
```

Normalize text input:

```python
answer = input("Continue? [yes/no]: ").strip().lower()

if answer in ("yes", "y"):
    print("Continuing")
```

Read multiple values:

```python
values = input("Enter numbers: ").split()
numbers = [int(value) for value in values]
```

# Conditional Statements

```python
if condition:
    statement()
elif another_condition:
    another_statement()
else:
    fallback_statement()
```

```python
seat_count = 10
status = "Available" if seat_count > 0 else "Full"
```

Truthiness:

```python
if workshop_name:
    print("A workshop was provided.")

if not workshop_name:
    print("The workshop name is empty.")
```

Falsy values include `False`, `None`, `0`, `0.0`, `""`, and empty collections.

## Menu Pattern

```python
while True:
    print("1. View workshops")
    print("2. Register")
    print("3. Exit")

    choice = input("Choose: ").strip()

    if choice == "1":
        show_workshops()
    elif choice == "2":
        register()
    elif choice == "3":
        break
    else:
        print("Invalid choice.")
```

# Loops

## `for`

```python
for workshop in workshops:
    print(workshop)

for number in range(1, 6):
    print(number)
```

## `while`

```python
count = 0

while count < 3:
    print(count)
    count += 1
```

## Loop Controls

```python
for number in range(10):
    if number == 3:
        continue
    if number == 7:
        break
    print(number)
```

Python has no built-in `do-while` statement. Use `while True` with `break`:

```python
while True:
    value = input("Enter a value: ")

    if value:
        break
```

# Collections

## Lists

Ordered and mutable. Allows duplicates.

```python
workshops = ["HTML", "Python"]
workshops.append("FastAPI")
workshops.insert(0, "CSS")
workshops[0] = "JavaScript"
removed = workshops.pop()
workshops.remove("Python")
```

```python
squares = [number ** 2 for number in range(1, 6)]
even_numbers = [number for number in range(10) if number % 2 == 0]
```

## Tuples

Ordered and immutable.

```python
location = (28.45, 77.50)
single_item = ("OSDC",)

name, city = ("OSDC", "Noida")
```

## Sets

Unordered collection of unique values.

```python
frontend = {"HTML", "CSS", "JavaScript"}
backend = {"Python", "FastAPI"}

union = frontend | backend
intersection = frontend & backend
difference = frontend - backend
```

Create an empty set with `set()`, not `{}`.

## Dictionaries

Mutable key-value collections. Keys must be unique.

```python
club = {
    "name": "OSDC",
    "institution": "JIIT, Noida"
}

print(club["name"])
print(club.get("website", "Not available"))
club["focus"] = "Open Source Development"
club.pop("focus")
```

```python
for key, value in club.items():
    print(key, value)
```

## Ranges

```python
list(range(5))           # [0, 1, 2, 3, 4]
list(range(2, 10, 2))    # [2, 4, 6, 8]
```

# Functions

```python
def calculate_total(first_value: int, second_value: int) -> int:
    """Return the sum of two values."""
    return first_value + second_value


result = calculate_total(10, 20)
```

Default parameters:

```python
def greet(name="OSDC member"):
    return f"Welcome, {name}."
```

Keyword arguments:

```python
def describe_workshop(name, duration):
    return f"{name}: {duration} days"


describe_workshop(duration=2, name="Python")
```

Flexible arguments:

```python
def show_topics(*topics):
    for topic in topics:
        print(topic)


def show_details(**details):
    for key, value in details.items():
        print(key, value)
```

Avoid mutable defaults:

```python
def add_topic(topic, topics=None):
    if topics is None:
        topics = []
    topics.append(topic)
    return topics
```

# Exceptions

```python
try:
    value = int(input("Enter a number: "))
except ValueError:
    print("Invalid number")
else:
    print(value)
finally:
    print("This always runs")
```

Raise an exception when a value is invalid:

```python
if seats < 0:
    raise ValueError("Seats cannot be negative.")
```

# File Handling

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

Modes:

| Mode | Purpose |
|---|---|
| `r` | Read |
| `w` | Write and replace |
| `a` | Append |
| `x` | Create only if missing |
| `rb` | Read binary |
| `wb` | Write binary |

JSON:

```python
import json

with open("workshop.json", "w", encoding="utf-8") as file:
    json.dump(workshop, file, indent=2)

with open("workshop.json", encoding="utf-8") as file:
    workshop = json.load(file)
```

# Libraries and Imports

```python
import math
import numpy as np
import pandas as pd

from pathlib import Path
from math import sqrt
import matplotlib.pyplot as plt
```

Install a package:

```bash
pip install package-name
pip freeze > requirements.txt
```

Useful standard-library examples:

```python
import math

print(math.sqrt(25))
print(math.ceil(4.2))
print(math.floor(4.8))
```

Use aliases to keep long library names convenient:

```python
import numpy as np
import pandas as pd
```

# Object-Oriented Programming

```python
class Workshop:
    total_workshops = 0

    def __init__(self, name, seats):
        self.name = name
        self.seats = seats
        Workshop.total_workshops += 1

    def register(self):
        if self.seats > 0:
            self.seats -= 1
            return True
        return False


workshop = Workshop("Python Workshop", 40)
workshop.register()
```

Inheritance:

```python
class OnlineWorkshop(Workshop):
    def __init__(self, name, seats, link):
        super().__init__(name, seats)
        self.link = link
```

Property validation:

```python
class Workshop:
    def __init__(self, seats):
        self.seats = seats

    @property
    def seats(self):
        return self._seats

    @seats.setter
    def seats(self, value):
        if value < 0:
            raise ValueError("Seats cannot be negative.")
        self._seats = value
```

# C vs Python

| Concept | C | Python |
|---|---|---|
| Entry point | `main()` | Top-level code or `main()` |
| Output | `printf()` | `print()` |
| Input | `scanf()` | `input()` |
| Variables | Declared with a type | Dynamically typed |
| Blocks | `{}` | Indentation |
| Boolean values | Often `0` and `1` | `True` and `False` |
| String | Character array | `str` |
| Memory | Manual allocation possible | Automatic memory management |
| Loop | `for`, `while`, `do-while` | `for`, `while` |

Python has no built-in `do-while`; use `while True` and `break`.

# APIs and HTTP

An API allows software systems to communicate.

Common HTTP methods:

| Method | Purpose |
|---|---|
| `GET` | Read data |
| `POST` | Create data or trigger an action |
| `PUT` | Replace data |
| `PATCH` | Partially update data |
| `DELETE` | Delete data |

Common status codes:

| Code | Meaning |
|---|---|
| `200` | Success |
| `201` | Created |
| `204` | Success with no body |
| `400` | Bad request |
| `401` | Authentication required or failed |
| `403` | Forbidden |
| `404` | Not found |
| `422` | Validation failed |
| `500` | Server error |

A request commonly contains a method, URL, headers, query parameters, and body. A response contains a status code, headers, and body.

# FastAPI

Minimal application:

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/")
def root():
    return {"message": "Welcome to OSDC"}
```

Run it:

```bash
pip install "fastapi[standard]"
fastapi dev main.py
```

Documentation:

```text
http://127.0.0.1:8000/docs
http://127.0.0.1:8000/redoc
```

Path and query parameters:

```python
@app.get("/workshops/{workshop_id}")
def get_workshop(workshop_id: int, topic: str | None = None):
    return {"id": workshop_id, "topic": topic}
```

Request model:

```python
from pydantic import BaseModel, Field


class WorkshopCreate(BaseModel):
    title: str = Field(min_length=3)
    seats: int = Field(gt=0)


@app.post("/workshops", status_code=201)
def create_workshop(workshop: WorkshopCreate):
    return workshop
```

Error response:

```python
from fastapi import HTTPException

raise HTTPException(status_code=404, detail="Workshop not found")
```

CORS:

```python
from fastapi.middleware.cors import CORSMiddleware

app.add_middleware(
    CORSMiddleware,
    allow_origins=["http://localhost:5500"],
    allow_methods=["GET", "POST"],
    allow_headers=["Content-Type"],
)
```

# Frontend Integration

GET request from JavaScript:

```javascript
const response = await fetch("http://127.0.0.1:8000/api/workshops");
const data = await response.json();
console.log(data.workshops);
```

POST request:

```javascript
const response = await fetch("http://127.0.0.1:8000/api/workshops", {
  method: "POST",
  headers: {
    "Content-Type": "application/json"
  },
  body: JSON.stringify({
    title: "Python Workshop",
    topic: "backend",
    seats: 40
  })
});

if (!response.ok) {
  throw new Error("Request failed");
}
```

Useful frontend responsibilities:

- HTML: structure and forms
- CSS: presentation
- JavaScript: events, requests, JSON, and DOM updates
- FastAPI: routes, validation, application logic, and responses

Use `textContent` for untrusted text instead of inserting it with `innerHTML`.

# Demo API Flow

The OSDC demo API uses:

```text
main.py -> FastAPI server
 demo.py -> requests-based Python client
```

Typical flow:

```text
Client sends JSON
    -> FastAPI validates a Pydantic model
    -> route applies business rules
    -> server returns JSON
    -> client calls response.json()
```

Common demo endpoints:

| Method | Endpoint | Purpose |
|---|---|---|
| `GET` | `/` | Check the API |
| `GET` | `/data` | View all data |
| `GET` | `/search/{query}` | Search data |
| `POST` | `/login` | Register or log in |
| `POST` | `/upload` | Add data |
| `POST` | `/update` | Update owned data |
| `POST` | `/delete` | Delete owned data |

# Common Reminders

- `=` assigns; `==` compares.
- `input()` returns `str`.
- Use `is None` to check for `None`.
- Use `with open(...)` for files.
- Use `response.ok` after `fetch()`.
- Validate data on the backend even if the frontend validates it.
- Do not store passwords as plain text.
- Do not expose secrets in frontend JavaScript.
- Keep API URLs and configuration in environment-specific settings.
- Use virtual environments for Python projects.
- Add dependencies to `requirements.txt`.
