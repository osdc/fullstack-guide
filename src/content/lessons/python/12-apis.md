---
course: python
slug: apis
title: Python APIs
description: "Learn how APIs work, make HTTP requests, handle JSON data, and interact with external services using Python."
---

API (application programming interface) is a way for 2 software bodies/systems to communicate with each other, a layer between them, hence the word "interface".

## API in a Fullstack Setup

In our fullstack case, the layer between the frontend (the UI) and the backend (the actual server) is the API. When you click a button on the UI side, it sends what is called a **request**, you request whatever you clicked on, to the backend via the API. The backend then does the work (like looking something up in a database) and sends a **response** back through the API, which the frontend then shows on screen.

```
Frontend (UI)  --request-->   API   --request-->   Backend (Server + Database)
Frontend (UI)  <--response--  API   <--response--  Backend (Server + Database)
```

For example, when you open a weather app and it shows today's temperature:

1. The app (frontend) sends a request: "give me the weather for this city".
2. The request goes through the API to the weather server (backend).
3. The server finds the data and sends it back as a response.
4. The app takes that response and displays it nicely on the screen.

## A Simple Analogy

Think of a restaurant. You (the frontend) sit at the table and look at the menu. The kitchen (the backend) prepares the food. You can't just walk into the kitchen, so the **waiter** (the API) takes your order to the kitchen and brings the food back to you. You don't need to know how the kitchen works, only what you can order.

## Using Other People's API's

In many cases other companies allow you access to their API's (through an **API key**), which allows you to use programming commands to work with the backend directly instead of navigating the front end. For example, instead of opening a website and clicking around to see the weather, you can write a few lines of code that ask the weather company's server directly and get the data back, then use it however you like in your own program.

### What is an API Key?

An API key is a long unique string of characters that works like a password or an ID card. It tells the company who is sending the request, so they can allow it, keep track of how much you use, and block anyone misusing it.

```
API key example:  a1b2c3d4e5f6g7h8i9j0
```

Keep your API key private. Don't share it or upload it to places like GitHub, since anyone with the key can use your account's access.

## Parts of an API Request

- **Endpoint (URL):** the address you send the request to, like `https://api.example.com/weather`.
- **Method:** what kind of action you want to do (see below).
- **Headers:** extra information sent with the request, like your API key.
- **Parameters / Body:** the details of what you are asking for, like the city name.

### Common Methods

| Method | Meaning | Example |
|--------|---------|---------|
| GET | Get / read some data | Load your profile |
| POST | Send / create new data | Post a comment |
| PUT | Update existing data | Edit your profile |
| DELETE | Remove data | Delete a post |

## Parts of an API Response

The response usually comes back with two main things:

- **Status code:** a number telling you how it went.
- **Data:** the actual information asked for, most commonly in a format called **JSON**.

| Status Code | Meaning |
|-------------|---------|
| 200 | OK, success |
| 201 | Created successfully |
| 400 | Bad request, something is wrong with what you sent |
| 401 | Unauthorized, missing or wrong API key |
| 404 | Not found |
| 500 | Server error, problem on their side |

### JSON

JSON looks a lot like a python dictionary, which makes it easy to work with in python.

```
{
    "city": "Delhi",
    "temperature": 31,
    "condition": "Sunny"
}
```

## Using an API in Python

Python's `requests` library (see the Libraries file) makes this easy. Install it with `pip install requests`.

### A Simple GET Request

```
import requests

response = requests.get("https://api.example.com/weather?city=Delhi")

print(response.status_code)    # 200
data = response.json()         # converts the JSON into a python dictionary
print(data["temperature"])
```

### Sending an API Key

Most API's want the key sent along with the request, either in the headers or as a parameter.

```
import requests

url = "https://api.example.com/weather"
params = {"city": "Delhi", "key": "YOUR_API_KEY"}

response = requests.get(url, params=params)
print(response.json())
```

Or in the headers:

```
headers = {"Authorization": "Bearer YOUR_API_KEY"}
response = requests.get(url, headers=headers)
```

### A POST Request

```
import requests

new_post = {"title": "Hello", "body": "My first post"}
response = requests.post("https://api.example.com/posts", json=new_post)

print(response.status_code)    # 201 if created
```

### Checking for Errors

Always check whether the request actually worked before using the data.

```
response = requests.get("https://api.example.com/weather?city=Delhi")

if response.status_code == 200:
    data = response.json()
    print(data["temperature"])
else:
    print("Something went wrong:", response.status_code)
```

### Looping Over API Data

APIs often return lists of things, which pairs nicely with loops.

```
data = response.json()

for item in data["results"]:
    print(item["name"], "-", item["price"])
```

## Types of API's You Will Come Across

- **Web API's (REST):** the most common kind, accessed over the internet using URLs and methods like GET and POST, usually returning JSON.
- **Library API's:** the functions a library gives you. When you call `math.sqrt()`, you are using the math library's API.
- **Operating system API's:** how programs ask the OS to do things like open files or use the network.

## Famous Examples of API's

- **Google Maps API:** add maps and directions to your own app.
- **OpenWeather API:** get weather data.
- **Spotify API:** get song, artist and playlist information.
- **GitHub API:** work with repositories and users.
- **Payment API's (like Razorpay or Stripe):** accept payments on a website.

## Why API's Matter

- They let different systems work together without knowing each other's insides.
- They save time, since you can use existing services instead of building everything yourself.
- They keep things secure, since the outside world only gets access to what the API allows.
- They let the frontend and backend be built separately, as long as both agree on how to talk through the API.
