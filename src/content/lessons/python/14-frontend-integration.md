---
course: python
slug: frontend-integration
title: Python Frontend Integration
description: "Learn how to connect a frontend application with a Python backend and FastAPI."
---

A full-stack application connects a frontend interface to a backend API.

- **HTML** defines the page structure.
- **CSS** styles the page.
- **JavaScript** handles interaction and sends HTTP requests.
- **FastAPI** validates requests, runs backend logic, and returns responses.

The communication flow is:

```text
User interacts with the page
        |
        v
JavaScript sends an HTTP request
        |
        v
FastAPI validates and processes the request
        |
        v
FastAPI returns a JSON response
        |
        v
JavaScript updates the HTML page
```

# Project Structure

A small frontend and FastAPI project can be organized like this:

```text
full-stack-demo/
|-- backend/
|   |-- main.py
|   |-- requirements.txt
|-- frontend/
|   |-- index.html
|   |-- style.css
|   |-- script.js
```

Keep the frontend and backend in separate directories. During development, they may run on different ports and therefore have different origins.

# The FastAPI Backend

Create `backend/main.py`:

```python
from fastapi import FastAPI
from fastapi.middleware.cors import CORSMiddleware
from pydantic import BaseModel, Field

app = FastAPI(title="OSDC Workshop API")

app.add_middleware(
    CORSMiddleware,
    allow_origins=["http://localhost:5500"],
    allow_credentials=True,
    allow_methods=["GET", "POST"],
    allow_headers=["Content-Type"],
)


class WorkshopCreate(BaseModel):
    title: str = Field(min_length=3)
    topic: str = Field(min_length=2)
    seats: int = Field(gt=0)


workshops = [
    {
        "id": 1,
        "title": "Python and FastAPI",
        "topic": "backend",
        "seats": 40
    },
    {
        "id": 2,
        "title": "HTML, CSS, and JavaScript",
        "topic": "frontend",
        "seats": 35
    }
]


@app.get("/api/workshops")
def list_workshops():
    return {"workshops": workshops}


@app.post("/api/workshops", status_code=201)
def create_workshop(workshop: WorkshopCreate):
    new_workshop = {
        "id": len(workshops) + 1,
        **workshop.model_dump()
    }
    workshops.append(new_workshop)
    return new_workshop
```

The API exposes:

- `GET /api/workshops`: returns all workshops
- `POST /api/workshops`: validates and creates a workshop

The `/api` prefix makes it clear that these routes return API data rather than HTML pages.

# CORS

CORS stands for **Cross-Origin Resource Sharing**. Browsers restrict JavaScript from calling a different origin unless the backend explicitly allows it.

For example:

- Frontend: `http://localhost:5500`
- Backend: `http://127.0.0.1:8000`

These are different origins because their schemes, hosts, or ports differ.

FastAPI enables the frontend origin with `CORSMiddleware`:

```python
app.add_middleware(
    CORSMiddleware,
    allow_origins=["http://localhost:5500"],
    allow_credentials=True,
    allow_methods=["GET", "POST"],
    allow_headers=["Content-Type"],
)
```

During development, a simple frontend may run on another port. Update `allow_origins` to match the actual frontend address.

Avoid using `allow_origins=["*"]` in production when credentials or private data are involved. List only trusted frontend origins.

# The HTML Page

Create `frontend/index.html`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>OSDC Workshops</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <main class="container">
    <h1>OSDC Workshops</h1>
    <p>Open Source Developers Community, JIIT, Noida</p>

    <section>
      <h2>Available Workshops</h2>
      <p id="status" role="status"></p>
      <ul id="workshop-list"></ul>
    </section>

    <section>
      <h2>Add a Workshop</h2>
      <form id="workshop-form">
        <label for="title">Title</label>
        <input id="title" name="title" required minlength="3">

        <label for="topic">Topic</label>
        <input id="topic" name="topic" required minlength="2">

        <label for="seats">Seats</label>
        <input id="seats" name="seats" type="number" min="1" required>

        <button type="submit">Add Workshop</button>
      </form>
    </section>
  </main>

  <script src="script.js"></script>
</body>
</html>
```

Important elements have IDs so JavaScript can find and update them:

- `status` displays loading and error messages.
- `workshop-list` receives workshop elements.
- `workshop-form` handles new workshop submissions.

The HTML validation attributes provide quick browser-side checks. The backend must still validate the request because frontend validation can be bypassed.

# The CSS File

Create `frontend/style.css`:

```css
:root {
  color-scheme: light;
  font-family: Georgia, serif;
  color: #17324d;
  background: #eef4f1;
}

* {
  box-sizing: border-box;
}

body {
  margin: 0;
  min-width: 320px;
}

.container {
  width: min(720px, calc(100% - 2rem));
  margin: 3rem auto;
  padding: 2rem;
  background: #ffffff;
  border: 1px solid #c7d8d1;
  border-radius: 8px;
}

h1,
h2 {
  color: #155e63;
}

section {
  margin-top: 2rem;
}

#workshop-list {
  display: grid;
  gap: 0.75rem;
  padding: 0;
  list-style: none;
}

#workshop-list li {
  padding: 1rem;
  background: #e8f3ee;
  border-left: 4px solid #e07a5f;
}

form {
  display: grid;
  gap: 0.5rem;
}

input,
button {
  min-height: 2.5rem;
  padding: 0.5rem 0.75rem;
  font: inherit;
}

button {
  margin-top: 0.75rem;
  color: #ffffff;
  background: #155e63;
  border: 0;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background: #0d474b;
}

#status {
  min-height: 1.5rem;
}
```

CSS does not communicate with FastAPI. It only controls how the HTML interface looks. JavaScript is responsible for the network requests.

# Reading Data with `fetch()`

Create `frontend/script.js`:

```javascript
const API_URL = "http://127.0.0.1:8000/api";

const workshopList = document.querySelector("#workshop-list");
const statusMessage = document.querySelector("#status");

async function loadWorkshops() {
  statusMessage.textContent = "Loading workshops...";

  try {
    const response = await fetch(`${API_URL}/workshops`);

    if (!response.ok) {
      throw new Error(`Request failed with status ${response.status}`);
    }

    const data = await response.json();
    renderWorkshops(data.workshops);
    statusMessage.textContent = "";
  } catch (error) {
    statusMessage.textContent = "Could not load workshops.";
    console.error(error);
  }
}

function renderWorkshops(workshops) {
  workshopList.replaceChildren();

  for (const workshop of workshops) {
    const item = document.createElement("li");
    item.textContent = `${workshop.title} | ${workshop.topic} | ${workshop.seats} seats`;
    workshopList.append(item);
  }
}

loadWorkshops();
```

The request flow is:

1. `fetch()` sends a GET request to FastAPI.
2. `response.ok` checks whether the HTTP status indicates success.
3. `response.json()` converts the JSON response into a JavaScript object.
4. `renderWorkshops()` creates HTML elements from the returned data.
5. `replaceChildren()` refreshes the list without leaving stale content on the page.

# Sending Data with `fetch()`

Add this code to `script.js` to handle the form:

```javascript
const workshopForm = document.querySelector("#workshop-form");

workshopForm.addEventListener("submit", async (event) => {
  event.preventDefault();

  const formData = new FormData(workshopForm);
  const workshop = {
    title: formData.get("title"),
    topic: formData.get("topic"),
    seats: Number(formData.get("seats"))
  };

  try {
    const response = await fetch(`${API_URL}/workshops`, {
      method: "POST",
      headers: {
        "Content-Type": "application/json"
      },
      body: JSON.stringify(workshop)
    });

    const data = await response.json();

    if (!response.ok) {
      throw new Error(data.detail ?? "Could not create workshop.");
    }

    workshopForm.reset();
    statusMessage.textContent = "Workshop created successfully.";
    await loadWorkshops();
  } catch (error) {
    statusMessage.textContent = error.message;
    console.error(error);
  }
});
```

The important parts are:

- `event.preventDefault()` stops the browser's default full-page form submission.
- `method: "POST"` selects the HTTP method.
- `Content-Type: application/json` tells FastAPI that the body is JSON.
- `JSON.stringify()` converts a JavaScript object into JSON text.
- `response.json()` reads the JSON returned by FastAPI.
- `response.ok` must be checked because `fetch()` does not reject automatically for HTTP errors such as `400` or `422`.

# Handling FastAPI Validation Errors

If the request does not match the Pydantic model, FastAPI normally returns status `422` with details such as:

```json
{
  "detail": [
    {
      "loc": ["body", "seats"],
      "msg": "Input should be greater than 0",
      "type": "greater_than"
    }
  ]
}
```

The frontend should display a useful message without assuming every error has the same structure.

```javascript
function getErrorMessage(data) {
  if (typeof data.detail === "string") {
    return data.detail;
  }

  if (Array.isArray(data.detail)) {
    return data.detail
      .map((error) => error.msg)
      .join(" ");
  }

  return "The request could not be completed.";
}
```

Use it after parsing the response:

```javascript
const data = await response.json();

if (!response.ok) {
  throw new Error(getErrorMessage(data));
}
```

# Complete `script.js`

The following version combines loading, rendering, form submission, and error handling:

```javascript
const API_URL = "http://127.0.0.1:8000/api";

const workshopList = document.querySelector("#workshop-list");
const statusMessage = document.querySelector("#status");
const workshopForm = document.querySelector("#workshop-form");

function getErrorMessage(data) {
  if (typeof data.detail === "string") {
    return data.detail;
  }

  if (Array.isArray(data.detail)) {
    return data.detail.map((error) => error.msg).join(" ");
  }

  return "The request could not be completed.";
}

function renderWorkshops(workshops) {
  workshopList.replaceChildren();

  if (workshops.length === 0) {
    workshopList.textContent = "No workshops found.";
    return;
  }

  for (const workshop of workshops) {
    const item = document.createElement("li");
    item.textContent = `${workshop.title} | ${workshop.topic} | ${workshop.seats} seats`;
    workshopList.append(item);
  }
}

async function loadWorkshops() {
  statusMessage.textContent = "Loading workshops...";

  try {
    const response = await fetch(`${API_URL}/workshops`);
    const data = await response.json();

    if (!response.ok) {
      throw new Error(getErrorMessage(data));
    }

    renderWorkshops(data.workshops);
    statusMessage.textContent = "";
  } catch (error) {
    statusMessage.textContent = error.message;
  }
}

workshopForm.addEventListener("submit", async (event) => {
  event.preventDefault();

  const formData = new FormData(workshopForm);
  const workshop = {
    title: formData.get("title"),
    topic: formData.get("topic"),
    seats: Number(formData.get("seats"))
  };

  try {
    const response = await fetch(`${API_URL}/workshops`, {
      method: "POST",
      headers: {
        "Content-Type": "application/json"
      },
      body: JSON.stringify(workshop)
    });
    const data = await response.json();

    if (!response.ok) {
      throw new Error(getErrorMessage(data));
    }

    workshopForm.reset();
    statusMessage.textContent = "Workshop created successfully.";
    await loadWorkshops();
  } catch (error) {
    statusMessage.textContent = error.message;
  }
});

loadWorkshops();
```

# Running the Full-Stack Example

From the backend directory, install FastAPI and start the server:

```bash
pip install "fastapi[standard]"
fastapi dev main.py
```

From the frontend directory, serve the static files. Do not open `index.html` directly with a `file://` URL because browser security behavior differs from a real HTTP origin.

One simple option is Python's built-in static server:

```bash
python -m http.server 5500
```

Open this URL in the browser:

```text
http://localhost:5500
```

The page loads workshops from FastAPI and can submit new workshops.

# Security and Production Notes

The example is designed for learning. A production application should also:

- Restrict CORS to trusted origins.
- Authenticate protected endpoints.
- Validate data on the backend even when the frontend validates it.
- Avoid placing secrets in frontend JavaScript.
- Use HTTPS in production.
- Handle loading, error, and empty states.
- Avoid inserting untrusted values with `innerHTML`.
- Use a database instead of an in-memory list.
- Configure environment-specific API URLs.

The example uses `textContent` instead of `innerHTML` when displaying workshop data. This prevents returned text from being interpreted as HTML.

# Quick Reference

```javascript
const response = await fetch("http://127.0.0.1:8000/api/workshops");
const data = await response.json();
```

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
```

| Part | Responsibility |
|---|---|
| HTML | Page structure and form controls |
| CSS | Layout, colors, spacing, and responsive presentation |
| JavaScript | Events, HTTP requests, JSON parsing, and DOM updates |
| `fetch()` | Sends HTTP requests from the browser |
| FastAPI | Routes, validation, business logic, and JSON responses |
| Pydantic | Validates request data on the backend |
| CORS | Allows approved frontend origins |
| `response.ok` | Checks whether a fetch response succeeded |
| `response.json()` | Parses a JSON response |
| `JSON.stringify()` | Converts a JavaScript value to JSON text |
