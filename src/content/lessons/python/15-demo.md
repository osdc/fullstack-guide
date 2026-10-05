---
course: python
slug: demo-project
title: Demo Project
description: "Build a practical Python project that combines core programming concepts, libraries, APIs, and backend development."
---

The [OSDC demo-api repository](https://github.com/Kavoyaa/demo-api) is a small FastAPI project with a Python command-line client. It demonstrates how a client sends HTTP requests to a backend and reads JSON responses.

```text
demo-api/
|-- main.py
|-- demo.py
|-- requirements.txt
|-- README.md
```

- `main.py` defines the FastAPI server.
- `demo.py` is a terminal client using `requests`.
- `requirements.txt` installs `fastapi[standard]` and `requests`.

# Running the Demo

Create and activate a virtual environment:

```bash
python -m venv .venv
```

On Windows PowerShell:

```powershell
.venv\Scripts\Activate.ps1
```

On macOS or Linux:

```bash
source .venv/bin/activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Start the API from the repository directory:

```bash
fastapi dev
```

The API runs at:

```text
http://127.0.0.1:8000
```

Keep the server running. In a second terminal, run:

```bash
python demo.py
```

The client displays a menu and sends requests to the running API.

# Application State

The demo stores data in Python dictionaries:

```python
creds = {}
data = {}
```

The credentials dictionary maps usernames to passwords:

```text
username -> password
```

The data dictionary maps a heading to its description and owner:

```text
heading -> [description, username]
```

This data is stored only in memory. It disappears when the API restarts. A production application should use a database and should never store plain-text passwords.

# Request Models

`main.py` uses Pydantic models to describe and validate JSON request bodies.

```python
class LoginData(BaseModel):
    username: str
    password: str


class UploadData(LoginData):
    heading: str
    description: str


class UpdateData(LoginData):
    heading: str
    new_heading: str
    new_description: str


class DeleteData(LoginData):
    heading: str
```

The upload, update, and delete models inherit `username` and `password` from `LoginData`.

A valid upload body is:

```json
{
  "username": "osdc-member",
  "password": "example-password",
  "heading": "Python",
  "description": "A programming language used in the backend workshop."
}
```

If a required field is missing, FastAPI returns a validation error before the route runs.

# API Endpoints

## Root Endpoint

The root route checks whether the API is running:

```python
@app.get("/")
async def root():
    return {"message": "Hello World"}
```

Request:

```text
GET http://127.0.0.1:8000/
```

Response:

```json
{
  "message": "Hello World"
}
```

## View All Data

The public `/data` route returns all stored entries:

```python
@app.get("/data")
async def get_data():
    results = []

    for heading in data:
        description, username = data[heading]
        results.append([heading, description, username])

    return {"data": results}
```

Request:

```text
GET http://127.0.0.1:8000/data
```

Example response:

```json
{
  "data": [
    [
      "Python",
      "A programming language used in the backend workshop.",
      "osdc-member"
    ]
  ]
}
```

The demo uses lists inside the response list. A production API would usually use named objects because they are easier for frontend code to read:

```json
{
  "data": [
    {
      "heading": "Python",
      "description": "A programming language used in the backend workshop.",
      "username": "osdc-member"
    }
  ]
}
```

## Search Data

The `/search/{query}` route uses a path parameter and searches both headings and descriptions without considering letter case.

```python
@app.get("/search/{query}")
async def search_data(query: str):
    results = []

    for heading in data:
        description, username = data[heading]

        if (
            query.lower() in heading.lower()
            or query.lower() in description.lower()
        ):
            results.append([heading, description, username])

    return {"data": results}
```

Example request:

```text
GET http://127.0.0.1:8000/search/python
```

## Login and Registration

The `/login` route registers a new username or logs in an existing username:

```python
@app.post("/login")
async def login_user(data: LoginData):
    correct_password = creds.get(data.username)

    if correct_password is None:
        creds[data.username] = data.password
        return {"message": "Register successful"}

    if correct_password == data.password:
        return {"message": "Login successful"}

    raise HTTPException(status_code=401, detail="Password Incorrect")
```

The behavior is:

- A new username is registered automatically.
- The correct password logs in an existing username.
- The wrong password returns status `401`.

Example request:

```bash
curl -X POST http://127.0.0.1:8000/login \
  -H "Content-Type: application/json" \
  -d "{\"username\": \"osdc-member\", \"password\": \"example-password\"}"
```

This is a teaching example. A real application should hash passwords, store users in a database, and use a proper session or token system.

## Upload Data

The `/upload` route authenticates the user and stores a new entry:

```python
@app.post("/upload")
async def upload_data(new_data: UploadData):
    login(new_data.username, new_data.password)

    data[new_data.heading] = [
        new_data.description,
        new_data.username
    ]

    return {
        "message": "Data uploaded successfully",
        "data": data
    }
```

Request body:

```json
{
  "username": "osdc-member",
  "password": "example-password",
  "heading": "FastAPI",
  "description": "A Python framework for building APIs."
}
```

## Update Data

The `/update` route:

1. Validates credentials.
2. Checks that the original heading exists.
3. Checks that the authenticated user owns it.
4. Checks that a new heading is not already used.
5. Replaces the old dictionary entry.

```python
@app.post("/update")
async def update_data(new_data: UpdateData):
    login(new_data.username, new_data.password)

    if new_data.heading not in data:
        raise HTTPException(status_code=404, detail="Heading not found")

    description, username = data[new_data.heading]

    if username != new_data.username:
        raise HTTPException(status_code=403, detail="Not your heading")

    if (
        new_data.new_heading != new_data.heading
        and new_data.new_heading in data
    ):
        raise HTTPException(status_code=409, detail="Heading already exists")

    data.pop(new_data.heading)
    data[new_data.new_heading] = [
        new_data.new_description,
        new_data.username
    ]

    return {
        "message": "Data updated successfully",
        "data": data
    }
```

The route uses these status codes:

- `401`: invalid credentials
- `404`: original heading does not exist
- `403`: the user does not own the heading
- `409`: the new heading is already used

## Delete Data

The `/delete` route uses the same authentication and ownership checks before removing an entry:

```python
@app.post("/delete")
async def delete_data(delete_request: DeleteData):
    login(delete_request.username, delete_request.password)

    if delete_request.heading not in data:
        raise HTTPException(status_code=404, detail="Heading not found")

    description, username = data[delete_request.heading]

    if username != delete_request.username:
        raise HTTPException(status_code=403, detail="Not your heading")

    data.pop(delete_request.heading)

    return {
        "message": "Data deleted successfully",
        "data": data
    }
```

The demo uses `POST /delete` with a JSON body. A production API might use `DELETE /data/{heading}` and handle authentication separately.

# Authentication Helper

The `login()` helper centralizes credential validation:

```python
def login(username: str, password: str):
    correct_password = creds.get(username)

    if correct_password is None:
        raise HTTPException(status_code=401, detail="Incorrect Username!")

    if correct_password != password:
        raise HTTPException(status_code=401, detail="Incorrect Password!")
```

Upload, update, and delete call this helper instead of repeating credential checks. A larger FastAPI application could implement this behavior as a dependency using `Depends()`.

# The Python Client

`demo.py` uses `requests` and stores the server address in one constant:

```python
import requests

BASE_URL = "http://127.0.0.1:8000"
```

The client functions call these endpoints:

| Client function | Request | Endpoint |
|---|---|---|
| `upload_data()` | `POST` | `/upload` |
| `view_all()` | `GET` | `/data` |
| `view_by_heading()` | `GET` | `/data` |
| `update_data()` | `POST` | `/update` |
| `delete_data()` | `POST` | `/delete` |

The client converts JSON responses into Python values with `response.json()`:

```python
response = requests.get(f"{BASE_URL}/data")
items = response.json()["data"]
```

It checks the status code before displaying success or an error:

```python
if response.status_code == 200:
    print(response.json()["message"])
else:
    print(f"Error: {response.json()}")
```

The menu repeatedly asks for a choice and calls the matching function:

```python
while True:
    print("[1] Upload data")
    print("[2] View all data")
    print("[3] View data by heading")
    print("[4] Update data")
    print("[5] Delete data")
    print("[6] Exit")

    choice = input("Choose an option: ")

    if choice == "1":
        upload_data()
    elif choice == "2":
        view_all()
    elif choice == "3":
        view_by_heading()
    elif choice == "4":
        update_data()
    elif choice == "5":
        delete_data()
    elif choice == "6":
        break
    else:
        print("Invalid choice.")
```

The client collects input and sends HTTP requests. The server validates requests, applies rules, changes data, and returns responses.

# Important Client-Server Mismatch

The current `main.py` requires `username` and `password` in `UploadData`, `UpdateData`, and `DeleteData`. However, the current `demo.py` sends only the heading and description for upload, and only heading fields for update and delete.

The current client sends an upload body like this:

```json
{
  "heading": "Python",
  "description": "A programming language."
}
```

The current server model requires this instead:

```json
{
  "username": "osdc-member",
  "password": "example-password",
  "heading": "Python",
  "description": "A programming language."
}
```

Therefore, the checked-in client and server are currently out of sync. The client must collect credentials and include them in protected requests, or the server models and authentication behavior must be changed.

A corrected upload request would look like this:

```python
response = requests.post(
    f"{BASE_URL}/upload",
    json={
        "username": "osdc-member",
        "password": "example-password",
        "heading": heading,
        "description": description
    }
)
```

The same credentials must be added to update and delete request bodies.

# Request Flow

An upload request travels through the application like this:

```text
demo.py
  |
  | POST /upload with JSON
  v
FastAPI route
  |
  | Pydantic validates UploadData
  v
login() validates credentials
  |
  | data[heading] is updated
  v
JSON response
  |
  v
demo.py reads response.json()
```

This is the same basic pattern used when a JavaScript frontend calls a FastAPI backend with `fetch()`.

# Endpoint Summary

| Method | Endpoint | Authentication | Purpose |
|---|---|---|---|
| `GET` | `/` | No | Check that the API is running |
| `GET` | `/data` | No | Return all stored entries |
| `GET` | `/search/{query}` | No | Search headings and descriptions |
| `POST` | `/login` | Credentials in body | Register or log in a user |
| `POST` | `/upload` | Required in body | Create an entry |
| `POST` | `/update` | Required in body | Update an owned entry |
| `POST` | `/delete` | Required in body | Delete an owned entry |
```

The interactive documentation is available at:

```text
http://127.0.0.1:8000/docs
```
