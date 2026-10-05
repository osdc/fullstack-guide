---
course: python
slug: fastapi
title: FastAPI with Python
description: "Learn how to build modern REST APIs with Python and FastAPI."
---


FastAPI is a modern Python web framework for building APIs. It is commonly used to create backend services that communicate with frontend applications built with HTML, CSS, and JavaScript.

An API allows one program to communicate with another. A frontend can send a request to a FastAPI backend, and the backend can return data, usually as JSON.

A typical full-stack application has this flow:

```text
Browser frontend -> HTTP request -> FastAPI backend -> response
Browser frontend <- JSON response <- FastAPI backend
```

# Why FastAPI?

FastAPI provides:

- Simple route definitions
- Automatic request validation
- Automatic interactive API documentation
- Support for synchronous and asynchronous code
- Python type-hint integration
- High performance for web APIs
- Easy integration with databases and authentication systems

FastAPI uses Starlette for web functionality and Pydantic for data validation and serialization.

# Installation

Create a virtual environment for the project:

```bash
python -m venv .venv
```

Activate it on Windows PowerShell:

```powershell
.venv\Scripts\Activate.ps1
```

Activate it on macOS or Linux:

```bash
source .venv/bin/activate
```

Install FastAPI with its standard development dependencies:

```bash
pip install "fastapi[standard]"
```

The standard dependencies include a server command that can run FastAPI applications.

You can save installed dependencies in a file:

```bash
pip freeze > requirements.txt
```

A minimal `requirements.txt` can contain:

```text
fastapi[standard]
```

# Creating the First Application

Create a file named `main.py`:

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/")
def read_root():
    return {"message": "Welcome to OSDC FastAPI"}
```

This application contains:

- `FastAPI()`: creates the application object
- `@app.get("/")`: registers a GET route at `/`
- `read_root()`: handles requests to that route
- The returned dictionary: becomes a JSON response

# Running the Application

Run the application from the terminal:

```bash
fastapi dev main.py
```

The development server usually starts at:

```text
http://127.0.0.1:8000
```

You can also run the application directly with Uvicorn:

```bash
uvicorn main:app --reload
```

The command uses the format:

```text
uvicorn module_name:application_variable --reload
```

For `main.py` and `app = FastAPI()`, this becomes `main:app`.

The `--reload` option restarts the development server when source files change. Do not use automatic reload in production.

# Testing the Root Route

Open this URL in a browser:

```text
http://127.0.0.1:8000/
```

Response:

```json
{
  "message": "Welcome to OSDC FastAPI"
}
```

You can also use a command-line HTTP client:

```bash
curl http://127.0.0.1:8000/
```

# Automatic Documentation

FastAPI automatically generates interactive documentation from the application routes and type hints.

Swagger UI is available at:

```text
http://127.0.0.1:8000/docs
```

ReDoc is available at:

```text
http://127.0.0.1:8000/redoc
```

The OpenAPI schema is available as JSON at:

```text
http://127.0.0.1:8000/openapi.json
```

These pages are useful for testing endpoints and understanding the API contract.

# HTTP Methods

HTTP methods describe the action a client wants to perform.

| Method | Common purpose |
|---|---|
| `GET` | Read data |
| `POST` | Create data |
| `PUT` | Replace existing data |
| `PATCH` | Partially update data |
| `DELETE` | Remove data |

FastAPI uses decorators to connect HTTP methods and URL paths to Python functions.

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/workshops")
def list_workshops():
    return {"workshops": []}


@app.post("/workshops")
def create_workshop():
    return {"message": "Workshop created"}


@app.put("/workshops/{workshop_id}")
def replace_workshop(workshop_id: int):
    return {"message": f"Workshop {workshop_id} replaced"}


@app.delete("/workshops/{workshop_id}")
def delete_workshop(workshop_id: int):
    return {"message": f"Workshop {workshop_id} deleted"}
```

# Path Parameters

A path parameter is a variable part of the URL. Put its name inside braces.

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/workshops/{workshop_id}")
def get_workshop(workshop_id: int):
    return {
        "id": workshop_id,
        "title": "Python and FastAPI Workshop"
    }
```

A request to `/workshops/1` produces:

```json
{
  "id": 1,
  "title": "Python and FastAPI Workshop"
}
```

Because `workshop_id` is annotated as `int`, FastAPI validates and converts the path value.

A request to `/workshops/abc` produces a validation error because `abc` is not an integer.

## Path Order Matters

Declare a fixed path before a path parameter that could match the same text.

```python
@app.get("/workshops/search")
def search_workshops():
    return {"message": "Search workshops"}


@app.get("/workshops/{workshop_id}")
def get_workshop(workshop_id: int):
    return {"id": workshop_id}
```

If the dynamic route is declared first, the word `search` may be interpreted as a `workshop_id`.

# Query Parameters

Query parameters appear after `?` in a URL.

```text
/workshops?topic=python&limit=10
```

Define them as function parameters that are not part of the path.

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/workshops")
def list_workshops(topic: str | None = None, limit: int = 10):
    return {
        "topic": topic,
        "limit": limit
    }
```

A request to `/workshops?topic=python&limit=5` produces:

```json
{
  "topic": "python",
  "limit": 5
}
```

## Required Query Parameters

A query parameter without a default value is required.

```python
@app.get("/search")
def search_workshops(query: str):
    return {"query": query}
```

The client must call `/search?query=python`. A request without `query` receives a validation error.

## Optional Query Parameters

Use `None` as the default for an optional parameter.

```python
@app.get("/workshops")
def list_workshops(topic: str | None = None):
    if topic is None:
        return {"message": "Returning all workshops"}

    return {"message": f"Returning workshops about {topic}"}
```

# Request Bodies with Pydantic Models

A request body contains data sent by the client, commonly with a `POST`, `PUT`, or `PATCH` request.

Use a Pydantic model to describe and validate the expected JSON structure.

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()


class WorkshopCreate(BaseModel):
    title: str
    topic: str
    seats: int


@app.post("/workshops")
def create_workshop(workshop: WorkshopCreate):
    return {
        "message": "Workshop created",
        "workshop": workshop
    }
```

A valid request body:

```json
{
  "title": "Python and FastAPI",
  "topic": "backend",
  "seats": 40
}
```

FastAPI validates the body before calling the route function. It also converts the Pydantic model back into JSON in the response.

# Pydantic Field Validation

Use `Field` to add constraints and metadata.

```python
from fastapi import FastAPI
from pydantic import BaseModel, Field

app = FastAPI()


class WorkshopCreate(BaseModel):
    title: str = Field(min_length=3, max_length=100)
    topic: str = Field(min_length=2)
    seats: int = Field(gt=0, le=500)


@app.post("/workshops")
def create_workshop(workshop: WorkshopCreate):
    return workshop
```

This model requires:

- A title between 3 and 100 characters
- A topic with at least 2 characters
- A seat count greater than 0 and at most 500

Invalid data receives a structured validation response instead of entering the route function.

# Combining Path, Query, and Body Parameters

A route can use all three parameter types.

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()


class WorkshopUpdate(BaseModel):
    title: str
    seats: int


@app.put("/workshops/{workshop_id}")
def update_workshop(
    workshop_id: int,
    update: WorkshopUpdate,
    notify_members: bool = False
):
    return {
        "id": workshop_id,
        "title": update.title,
        "seats": update.seats,
        "notify_members": notify_members
    }
```

In this example:

- `workshop_id` is a path parameter
- `update` is a JSON request body
- `notify_members` is a query parameter

# Response Models

A response model defines the data that an endpoint should return. It documents the response and filters out fields that should not be exposed.

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()


class WorkshopResponse(BaseModel):
    id: int
    title: str
    topic: str
    seats: int


@app.get("/workshops/{workshop_id}", response_model=WorkshopResponse)
def get_workshop(workshop_id: int):
    return {
        "id": workshop_id,
        "title": "Python and FastAPI Workshop",
        "topic": "backend",
        "seats": 40,
        "internal_note": "This field is not in the response model."
    }
```

The `internal_note` field is excluded from the response because it is not part of `WorkshopResponse`.

Separating input and output models is usually clearer:

```python
class WorkshopCreate(BaseModel):
    title: str
    topic: str
    seats: int


class WorkshopResponse(BaseModel):
    id: int
    title: str
    topic: str
    seats: int
```

# Status Codes

HTTP status codes communicate the result of a request.

| Status code | Meaning |
|---|---|
| `200` | Successful request |
| `201` | Resource created |
| `204` | Successful request with no response body |
| `400` | Invalid request |
| `401` | Authentication required |
| `403` | Access forbidden |
| `404` | Resource not found |
| `422` | Validation failed |
| `500` | Server-side error |

Set a default status code with `status_code`:

```python
from fastapi import FastAPI, status

app = FastAPI()


@app.post("/workshops", status_code=status.HTTP_201_CREATED)
def create_workshop():
    return {"message": "Workshop created"}
```

# Handling Errors with `HTTPException`

Raise `HTTPException` when a requested resource cannot be found or an action is not allowed.

```python
from fastapi import FastAPI, HTTPException

app = FastAPI()

workshops = {
    1: {
        "id": 1,
        "title": "Python and FastAPI Workshop"
    }
}


@app.get("/workshops/{workshop_id}")
def get_workshop(workshop_id: int):
    workshop = workshops.get(workshop_id)

    if workshop is None:
        raise HTTPException(
            status_code=404,
            detail="Workshop not found"
        )

    return workshop
```

The client receives a response such as:

```json
{
  "detail": "Workshop not found"
}
```

# In-Memory CRUD Example

CRUD means:

- **Create**
- **Read**
- **Update**
- **Delete**

The following example stores data in a Python dictionary. It is useful for learning routes, but data will be lost when the server restarts.

```python
from fastapi import FastAPI, HTTPException, status
from pydantic import BaseModel, Field

app = FastAPI(title="OSDC Workshop API")


class WorkshopCreate(BaseModel):
    title: str = Field(min_length=3)
    topic: str = Field(min_length=2)
    seats: int = Field(gt=0)


class WorkshopResponse(WorkshopCreate):
    id: int


workshops: dict[int, WorkshopResponse] = {}
next_id = 1


@app.get("/workshops", response_model=list[WorkshopResponse])
def list_workshops():
    return list(workshops.values())


@app.get("/workshops/{workshop_id}", response_model=WorkshopResponse)
def get_workshop(workshop_id: int):
    workshop = workshops.get(workshop_id)

    if workshop is None:
        raise HTTPException(status_code=404, detail="Workshop not found")

    return workshop


@app.post(
    "/workshops",
    response_model=WorkshopResponse,
    status_code=status.HTTP_201_CREATED
)
def create_workshop(workshop: WorkshopCreate):
    global next_id

    new_workshop = WorkshopResponse(
        id=next_id,
        **workshop.model_dump()
    )
    workshops[next_id] = new_workshop
    next_id += 1

    return new_workshop


@app.delete("/workshops/{workshop_id}", status_code=status.HTTP_204_NO_CONTENT)
def delete_workshop(workshop_id: int):
    if workshop_id not in workshops:
        raise HTTPException(status_code=404, detail="Workshop not found")

    del workshops[workshop_id]
```

A real application would store workshops in a database instead of an in-memory dictionary.

# Async Routes

FastAPI supports both normal functions and asynchronous functions.

Use `async def` when the route performs asynchronous operations, such as calling an async database driver or another async service.

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/status")
async def get_status():
    return {"status": "online"}
```

Use normal `def` for regular synchronous code. Do not add `async` unless the function actually benefits from asynchronous operations.

An asynchronous operation must be awaited:

```python
import asyncio
from fastapi import FastAPI

app = FastAPI()


@app.get("/delayed-status")
async def get_delayed_status():
    await asyncio.sleep(1)
    return {"status": "online"}
```

# Dependencies

Dependencies allow shared logic to be declared once and reused by multiple routes. Use `Depends` from FastAPI.

```python
from typing import Annotated

from fastapi import Depends, FastAPI

app = FastAPI()


def get_current_club():
    return {
        "name": "OSDC",
        "institution": "JIIT, Noida"
    }


ClubDependency = Annotated[dict, Depends(get_current_club)]


@app.get("/club")
def get_club(club: ClubDependency):
    return club
```

Dependencies are useful for:

- Authentication and authorization
- Database sessions
- Shared query parameters
- Common request checks
- Reusable configuration

A simple dependency can also return a value based on a query parameter:

```python
from fastapi import Depends, FastAPI

app = FastAPI()


def pagination(skip: int = 0, limit: int = 10):
    return {"skip": skip, "limit": limit}


@app.get("/workshops")
def list_workshops(page=Depends(pagination)):
    return page
```

# Routers and Project Structure

As an application grows, place related routes in separate modules.

A common structure is:

```text
project/
|-- app/
|   |-- __init__.py
|   |-- main.py
|   |-- models.py
|   |-- schemas.py
|   |-- dependencies.py
|   |-- routers/
|       |-- __init__.py
|       |-- workshops.py
|       |-- members.py
|-- requirements.txt
```

Create a router in `app/routers/workshops.py`:

```python
from fastapi import APIRouter

router = APIRouter(prefix="/workshops", tags=["workshops"])


@router.get("/")
def list_workshops():
    return {"workshops": []}
```

Include it in `app/main.py`:

```python
from fastapi import FastAPI

from app.routers import workshops

app = FastAPI(title="OSDC API")
app.include_router(workshops.router)
```

The route is now available at `/workshops/`.

Routers help keep `main.py` small and organize endpoints by feature.

# CORS and Frontend Requests

Browsers enforce the same-origin policy. If the frontend and backend use different origins, the backend must allow the frontend origin through CORS.

For example:

- Frontend: `http://localhost:5500`
- Backend: `http://127.0.0.1:8000`

These are different origins.

Configure CORS with `CORSMiddleware`:

```python
from fastapi import FastAPI
from fastapi.middleware.cors import CORSMiddleware

app = FastAPI()

app.add_middleware(
    CORSMiddleware,
    allow_origins=["http://localhost:5500"],
    allow_credentials=True,
    allow_methods=["GET", "POST", "PUT", "DELETE"],
    allow_headers=["Content-Type", "Authorization"],
)


@app.get("/api/message")
def get_message():
    return {"message": "Hello from OSDC FastAPI"}
```

During development, you may temporarily allow all origins:

```python
app.add_middleware(
    CORSMiddleware,
    allow_origins=["*"],
    allow_credentials=False,
    allow_methods=["*"],
    allow_headers=["*"],
)
```

In production, list only trusted frontend origins instead of allowing every origin.

# Connecting a JavaScript Frontend

A browser can call a FastAPI endpoint with the JavaScript `fetch()` function.

FastAPI route:

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/api/workshops")
def list_workshops():
    return {
        "workshops": [
            {"id": 1, "title": "Python Workshop"},
            {"id": 2, "title": "FastAPI Workshop"}
        ]
    }
```

Frontend JavaScript:

```javascript
async function loadWorkshops() {
  const response = await fetch("http://127.0.0.1:8000/api/workshops");

  if (!response.ok) {
    throw new Error("Unable to load workshops");
  }

  const data = await response.json();
  console.log(data.workshops);
}

loadWorkshops();
```

The flow is:

1. JavaScript sends a GET request.
2. FastAPI runs the route function.
3. FastAPI returns a JSON response.
4. JavaScript converts the response with `response.json()`.
5. The frontend uses the returned data to update the page.

# Sending JSON from JavaScript

A frontend can send JSON to a FastAPI `POST` endpoint.

FastAPI backend:

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()


class WorkshopCreate(BaseModel):
    title: str
    topic: str
    seats: int


@app.post("/api/workshops")
def create_workshop(workshop: WorkshopCreate):
    return {
        "message": "Workshop received",
        "workshop": workshop
    }
```

Frontend JavaScript:

```javascript
async function createWorkshop() {
  const response = await fetch("http://127.0.0.1:8000/api/workshops", {
    method: "POST",
    headers: {
      "Content-Type": "application/json"
    },
    body: JSON.stringify({
      title: "Python and FastAPI",
      topic: "backend",
      seats: 40
    })
  });

  const data = await response.json();
  console.log(data);
}

createWorkshop();
```

The `Content-Type` header tells the backend that the request body contains JSON.

# Returning HTML or JSON

FastAPI is commonly used for JSON APIs, but it can also return other response types.

```python
from fastapi import FastAPI
from fastapi.responses import HTMLResponse

app = FastAPI()


@app.get("/welcome", response_class=HTMLResponse)
def welcome_page():
    return "<h1>Welcome to OSDC</h1>"
```

For a separate HTML, CSS, and JavaScript frontend, JSON responses are usually the better choice.

# Environment Variables

Configuration such as database URLs and secret keys should not be hard-coded in source files.

A simple `.env` file might contain:

```text
APP_ENV=development
DATABASE_URL=sqlite:///./osdc.db
```

Use a settings model to read configuration. Install the settings package if needed:

```bash
pip install pydantic-settings
```

```python
from pydantic_settings import BaseSettings, SettingsConfigDict


class Settings(BaseSettings):
    app_env: str = "development"
    database_url: str

    model_config = SettingsConfigDict(env_file=".env")


settings = Settings()
print(settings.app_env)
```

Do not commit `.env` files containing passwords, tokens, or other secrets.

# Database Integration

FastAPI does not require a specific database. It can work with relational and NoSQL databases through Python libraries.

Common choices include:

- SQLite for small local applications
- PostgreSQL for production relational applications
- MySQL for relational applications
- MongoDB for document-oriented applications

A typical database-backed route has this flow:

```text
Request -> validation -> database session -> query -> response model -> JSON
```

Keep database code separate from route definitions when possible. This makes the application easier to test and maintain.

# Authentication Overview

Authentication verifies who a user is. Authorization checks what that user is allowed to do.

Common API authentication approaches include:

- Session cookies
- API keys
- OAuth2
- Bearer tokens
- JSON Web Tokens

FastAPI provides security utilities such as OAuth2 helpers, but authentication must still be designed and configured correctly for the application.

Never place passwords or secret tokens directly in source code.

# Testing an API

FastAPI's `TestClient` can test routes without starting a real development server.

Install the test dependencies if needed:

```bash
pip install pytest httpx
```

Example application:

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/api/message")
def get_message():
    return {"message": "Hello from OSDC"}
```

Test file:

```python
from fastapi.testclient import TestClient

from main import app

client = TestClient(app)


def test_get_message():
    response = client.get("/api/message")

    assert response.status_code == 200
    assert response.json() == {"message": "Hello from OSDC"}
```

Run tests with:

```bash
pytest
```

# Production Considerations

Development and production have different requirements. Before deploying an API:

- Disable development auto-reload
- Configure trusted CORS origins
- Store secrets in environment variables or a secret manager
- Use a production database
- Add authentication and authorization where required
- Validate all external input
- Return suitable status codes
- Add logging and monitoring
- Write automated tests
- Run behind a suitable process manager or hosting platform

A production server can be started with Uvicorn without `--reload`:

```bash
uvicorn main:app --host 0.0.0.0 --port 8000
```

The exact deployment command depends on the hosting environment.

# Complete Beginner API

The following single-file application demonstrates a small OSDC workshop API.

```python
from fastapi import FastAPI, HTTPException, status
from pydantic import BaseModel, Field

app = FastAPI(title="OSDC Workshop API")


class WorkshopCreate(BaseModel):
    title: str = Field(min_length=3)
    topic: str = Field(min_length=2)
    seats: int = Field(gt=0)


class Workshop(WorkshopCreate):
    id: int


workshops: list[Workshop] = []


@app.get("/")
def read_root():
    return {"message": "Welcome to the OSDC Workshop API"}


@app.get("/workshops", response_model=list[Workshop])
def list_workshops(topic: str | None = None):
    if topic is None:
        return workshops

    return [
        workshop
        for workshop in workshops
        if workshop.topic.lower() == topic.lower()
    ]


@app.get("/workshops/{workshop_id}", response_model=Workshop)
def get_workshop(workshop_id: int):
    for workshop in workshops:
        if workshop.id == workshop_id:
            return workshop

    raise HTTPException(status_code=404, detail="Workshop not found")


@app.post(
    "/workshops",
    response_model=Workshop,
    status_code=status.HTTP_201_CREATED
)
def create_workshop(workshop_data: WorkshopCreate):
    next_id = len(workshops) + 1
    workshop = Workshop(id=next_id, **workshop_data.model_dump())
    workshops.append(workshop)
    return workshop
```

Run it with:

```bash
fastapi dev main.py
```

Then open the interactive documentation at `/docs` and try the endpoints.

# Quick Reference

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()


class WorkshopCreate(BaseModel):
    title: str
    seats: int


@app.get("/workshops/{workshop_id}")
def get_workshop(workshop_id: int):
    return {"id": workshop_id}


@app.post("/workshops")
def create_workshop(workshop: WorkshopCreate):
    return workshop
```

| FastAPI concept | Purpose |
|---|---|
| `FastAPI()` | Creates the application |
| `@app.get()` | Registers a GET endpoint |
| `@app.post()` | Registers a POST endpoint |
| Path parameter | Reads a value from the URL path |
| Query parameter | Reads a value after `?` in the URL |
| Pydantic model | Validates structured request data |
| `response_model` | Defines and filters response data |
| `HTTPException` | Returns an HTTP error response |
| `status_code` | Sets the endpoint's default status code |
| `Depends()` | Injects reusable dependencies |
| `CORSMiddleware` | Allows approved frontend origins |
| `async def` | Defines an asynchronous route |
| `/docs` | Opens Swagger UI documentation |
| `/redoc` | Opens ReDoc documentation |
