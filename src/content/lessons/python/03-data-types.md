---
course: python
slug: data-types
title: Data Types
description: "Learn Python's basic data types, type conversion, and how values are represented in programs."
---

Python data types define the kind of value stored in a variable and the operations that can be performed on it.

Python is **dynamically typed**, so you do not need to declare a variable's type explicitly.

```python
club_name = "OSDC"       # string
member_count = 50        # integer
event_rating = 4.8       # float
is_cool = True           # boolean
```

The type can be checked using `type()`.

```python
value = 42

print(type(value))
# <class 'int'>
```

## Variables and Assignment

A variable is a name referring to a value.

```python
club_name = "OSDC"
club_location = "JIIT, Noida"
is_open_source = True

print(club_name)
print(club_location)
print(is_open_source)
```

Python allows assigning multiple variables at once:

```python
html, css, javascript = "HTML", "CSS", "JavaScript"
```

The same value can be assigned to multiple variables:

```python
first, second, third = "OSDC"
```

Variable names:

- Can contain letters, numbers, and underscores
- Cannot start with a number
- Are case-sensitive
- Cannot be Python keywords

```python
club_name = "OSDC"  # valid
_member_count = 50  # valid
# 2_members = 50    # invalid
```

## Built-in Data Types

Python's commonly used built-in data types are:

| Category | Data Types |
|---|---|
| Numeric | `int`, `float`, `complex` |
| Boolean | `bool` |
| Text | `str` |
| Sequence | `list`, `tuple`, `range` |
| Mapping | `dict` |
| Set | `set`, `frozenset` |
| Binary | `bytes`, `bytearray` |
| Empty value | `NoneType` |

# Numeric Types

## Integers

Integers represent whole numbers, including negative numbers and zero.

```python
member_count = 50
workshop_days = 2
remaining_seats = 0

print(member_count)
print(type(member_count))
```

Python integers can be arbitrarily large:

```python
large_number = 999999999999999999999999999999
print(large_number)
```

## Floating-Point Numbers

Floating-point numbers represent decimal values.

```python
event_rating = 4.8
temperature = -2.5

print(event_rating)
print(type(event_rating))
```

Floating-point calculations may have small precision errors:

```python
result = 0.1 + 0.2
print(result)
# 0.30000000000000004
```

For highly accurate decimal calculations, use the `decimal` module.

```python
from decimal import Decimal

result = Decimal("0.1") + Decimal("0.2")
print(result)
# 0.3
```

## Complex Numbers

Complex numbers contain a real and an imaginary part.

```python
number = 3 + 4j

print(number.real)
print(number.imag)
print(type(number))
```

The imaginary part uses `j`, not `i`.

# Boolean Type

The Boolean type has only two values:

```python
True
False
```

Booleans are commonly used in conditions.

```python
registration_open = True

if registration_open:
    print("OSDC registrations are open.")
else:
    print("Registrations are closed.")
```

The following values are considered false in conditions:

- `False`
- `None`
- `0`
- `0.0`
- `""`
- Empty collections such as `[]`, `{}`, `()`, and `set()`

Most other values are considered true.

```python
print(bool(0))        # False
print(bool(1))        # True
print(bool(""))       # False
print(bool("OSDC"))   # True
print(bool([]))       # False
print(bool([1, 2]))   # True
```

# Strings

A string is a sequence of characters enclosed in single, double, or triple quotes.

```python
single_quoted = 'OSDC'
double_quoted = "Open Source Development"
multi_line = """OSDC is a club at
JIIT, Noida."""
```

Strings support indexing and slicing.

```python
club_name = "OSDC"

print(club_name[0])    # O
print(club_name[-1])   # C
print(club_name[0:3])  # OSD
print(club_name[:2])   # OS
print(club_name[2:])   # DC
```

Strings are immutable. Their individual characters cannot be changed directly.

```python
club_name = "OSDC"

# club_name[0] = "A"  # TypeError
club_name = "A" + club_name[1:]
print(club_name)
```

Common string operations:

```python
message = "  Welcome to OSDC!  "

print(len(message))
print(message.strip())
print(message.lower())
print(message.upper())
print(message.replace("OSDC", "Open Source Development Community"))
print(message.startswith("  Welcome"))
print(message.endswith("  "))
```

Splitting and joining strings:

```python
technologies = "HTML,CSS,JavaScript,Python"

technology_list = technologies.split(",")
print(technology_list)

result = " - ".join(technology_list)
print(result)
```

Membership testing:

```python
description = "OSDC promotes open source development at JIIT, Noida."

print("open source" in description)
print("Java" not in description)
```

# Lists

A list is an ordered and mutable collection. Lists can contain values of different types.

```python
events = ["Web Development Workshop", "Git Workshop", "Python Workshop"]
mixed = [50, "OSDC", True, 4.8]

print(events)
print(mixed)
```

Accessing list elements:

```python
events = ["Web Development Workshop", "Git Workshop", "Python Workshop"]

print(events[0])
print(events[-1])
print(events[0:2])
```

Modifying lists:

```python
events = ["Git Workshop", "Python Workshop"]

events.append("FastAPI Workshop")
events.insert(1, "Web Development Workshop")
events[0] = "Git and GitHub Workshop"

print(events)
```

Removing elements:

```python
workshops = ["HTML Workshop", "CSS Workshop", "JavaScript Workshop"]

workshops.remove("CSS Workshop")
last_workshop = workshops.pop()
del workshops[0]

print(workshops)
print(last_workshop)
```

Useful list methods:

```python
attendance = [40, 25, 35, 30]

attendance.sort()
print(attendance)

attendance.reverse()
print(attendance)

print(len(attendance))
print(sum(attendance))
print(min(attendance))
print(max(attendance))
```

Lists can be copied using slicing or `.copy()`.

```python
original = ["HTML", "CSS", "JavaScript"]
copy = original.copy()

copy.append("Python")

print(original)
# ['HTML', 'CSS', 'JavaScript']

print(copy)
# ['HTML', 'CSS', 'JavaScript', 'Python']
```

## List Comprehensions

A list comprehension provides a short way to create lists.

```python
workshop_numbers = [number ** 2 for number in range(1, 6)]
print(workshop_numbers)
```

With a condition:

```python
even_numbers = [number for number in range(1, 11) if number % 2 == 0]
print(even_numbers)
```

# Tuples

A tuple is an ordered and immutable collection.

```python
club_location = (28.45, 77.50)
club_details = ("OSDC", "JIIT, Noida", "Open Source Development")

print(club_location[0])
print(club_details)
```

Tuples cannot be modified after creation.

```python
club_details = ("OSDC", "JIIT, Noida", "Open Source Development")

# club_details[0] = "New Club"  # TypeError
```

A tuple with one element requires a trailing comma:

```python
single_item_tuple = ("OSDC",)
not_a_tuple = ("OSDC")

print(type(single_item_tuple))
print(type(not_a_tuple))
```

Tuple unpacking:

```python
club_details = ("OSDC", "JIIT, Noida", "Open Source Development")

club_name, location, focus = club_details

print(club_name)
print(location)
print(focus)
```

Tuples are useful for fixed collections of related values.

# Sets

A set is an unordered collection of unique values.

```python
technologies = {"HTML", "CSS", "JavaScript", "JavaScript"}

print(technologies)
# {'HTML', 'CSS', 'JavaScript'}
```

Creating an empty set:

```python
empty_set = set()
empty_dictionary = {}

print(type(empty_set))
print(type(empty_dictionary))
```

Set operations:

```python
frontend = {"HTML", "CSS", "JavaScript"}
backend = {"Python", "JavaScript", "FastAPI"}

print(frontend | backend)  # union
print(frontend & backend)  # intersection
print(frontend - backend)  # difference
print(frontend ^ backend)  # symmetric difference
```

Useful methods:

```python
technologies = {"HTML", "CSS"}

technologies.add("Python")
technologies.update(["JavaScript", "FastAPI"])
technologies.discard("Java")

print(technologies)
```

Sets are useful for removing duplicate values and performing mathematical set operations.

```python
workshop_ids = [101, 102, 102, 103, 103, 103]
unique_workshop_ids = list(set(workshop_ids))

print(unique_workshop_ids)
```

# Dictionaries

A dictionary stores data as key-value pairs.

```python
club = {
    "name": "OSDC",
    "institution": "JIIT, Noida",
    "focus": "Open Source Development"
}

print(club["name"])
print(club["institution"])
```

Keys must be unique and immutable types such as strings, numbers, or tuples.

Accessing values safely:

```python
print(club.get("name"))
print(club.get("website", "Website not available"))
```

Adding and updating values:

```python
club["focus"] = "Open Source and Web Development"
club["active_since"] = 2016

print(club)
```

Removing values:

```python
club.pop("active_since")
print(club)

# del club["focus"]
```

Looping through a dictionary:

```python
club = {
    "name": "OSDC",
    "institution": "JIIT, Noida",
    "focus": "Open Source Development"
}

for key, value in club.items():
    print(key, ":", value)
```

Useful dictionary methods:

```python
print(club.keys())
print(club.values())
print(club.items())
```

Dictionaries are commonly used for JSON data and API requests.

```python
workshop = {
    "id": 1,
    "title": "Python and FastAPI Workshop",
    "is_active": True,
    "topics": ["HTML", "CSS", "JavaScript", "Python", "FastAPI"]
}
```

# Range

`range()` represents a sequence of numbers, usually used in loops.

```python
print(list(range(5)))
# [0, 1, 2, 3, 4]

print(list(range(2, 10, 2)))
# [2, 4, 6, 8]
```

Syntax:

```python
range(start, stop, step)
```

The `stop` value is excluded.

# None

`None` represents the absence of a value.

```python
registration_status = None

if registration_status is None:
    print("Registration status is not available.")
```

`None` is different from `0`, `False`, and an empty string.

```python
print(None == 0)      # False
print(None == False)  # False
```

Use `is None` when checking for `None`.

# Type Conversion

Type conversion changes a value from one data type to another.

```python
member_count = "50"

integer_value = int(member_count)
float_value = float(member_count)
string_value = str(integer_value)

print(integer_value)
print(float_value)
print(string_value)
```

Common conversion functions:

| Function | Converts to |
|---|---|
| `int()` | Integer |
| `float()` | Floating-point number |
| `str()` | String |
| `bool()` | Boolean |
| `list()` | List |
| `tuple()` | Tuple |
| `set()` | Set |
| `dict()` | Dictionary, when applicable |

Example:

```python
workshop_ids = (101, 102, 102, 103)

print(list(workshop_ids))
print(set(workshop_ids))
print(tuple([104, 105, 106]))
```

Invalid conversions raise an error:

```python
# value = int("OSDC")  # ValueError
```

Use exception handling for user input:

```python
user_input = "OSDC"

try:
    member_count = int(user_input)
    print(member_count)
except ValueError:
    print("Please enter a valid member count.")
```

# Mutable and Immutable Types

## Immutable Types

Immutable values cannot be changed after creation.

Examples:

- `int`
- `float`
- `bool`
- `str`
- `tuple`
- `frozenset`

```python
club_name = "OSDC"
club_name = club_name.lower()

print(club_name)
```

The original string was not changed. A new string was created.

## Mutable Types

Mutable values can be changed after creation.

Examples:

- `list`
- `dict`
- `set`
- `bytearray`

```python
technologies = ["HTML", "CSS", "JavaScript"]
technologies.append("Python")

print(technologies)
```

# Copying Data

Assignment creates another reference to the same object.

```python
first = ["HTML", "CSS", "JavaScript"]
second = first

second.append("Python")

print(first)
# ['HTML', 'CSS', 'JavaScript', 'Python']
```

Use `.copy()` for a shallow copy:

```python
first = ["HTML", "CSS", "JavaScript"]
second = first.copy()

second.append("Python")

print(first)
# ['HTML', 'CSS', 'JavaScript']

print(second)
# ['HTML', 'CSS', 'JavaScript', 'Python']
```

For nested objects, use `deepcopy()`:

```python
from copy import deepcopy

original = [["HTML", "CSS"], ["Python", "FastAPI"]]
copied = deepcopy(original)

copied[0].append("JavaScript")

print(original)
print(copied)
```

# Identity and Equality

`==` checks whether two values are equal.

`is` checks whether two variables refer to the same object.

```python
first = ["HTML", "CSS"]
second = ["HTML", "CSS"]
third = first

print(first == second)  # True
print(first is second)  # False
print(first is third)   # True
```

Use `==` for value comparison and `is` mainly for checking `None`.

# Nested Data Structures

Python data types can be placed inside other data types.

```python
workshops = [
    {
        "title": "Web Development Workshop",
        "organizer": "OSDC",
        "topics": ["HTML", "CSS", "JavaScript"]
    },
    {
        "title": "Python and FastAPI Workshop",
        "organizer": "OSDC",
        "topics": ["Python", "FastAPI"]
    }
]

print(workshops[0]["title"])
print(workshops[1]["topics"][0])
```

This structure is similar to JSON and is frequently used while building APIs.

# Type Hints

Type hints document the expected type of a variable. Python does not enforce them automatically.

```python
club_name: str = "OSDC"
member_count: int = 50
event_rating: float = 4.8
is_active: bool = True
```

Type hints can also be used with collections:

```python
technologies: list[str] = ["HTML", "CSS", "JavaScript", "Python"]

workshop_attendance: dict[str, int] = {
    "Web Development": 40,
    "Python and FastAPI": 50
}
```

They improve readability and help code editors detect mistakes.

# Checking Types

Use `type()` to get the exact type:

```python
value = 10
print(type(value))
```

Use `isinstance()` to check whether a value belongs to a type:

```python
value = 10

print(isinstance(value, int))
print(isinstance(value, (int, float)))
```

`isinstance()` is generally preferred for type checks.

# Common Mistakes

## Confusing `=` and `==`

```python
member_count = 50       # assignment
print(member_count == 50)  # comparison
```

## Modifying an Immutable Value

```python
club_name = "OSDC"

# club_name[0] = "A"  # TypeError
```

## Using a Mutable Default Value

Avoid using a list or dictionary as a default function argument:

```python
# Avoid this:
def add_topic(topic, topics=[]):
    topics.append(topic)
    return topics
```

Use `None` instead:

```python
def add_topic(topic, topics=None):
    if topics is None:
        topics = []

    topics.append(topic)
    return topics
```

## Unexpected Type Mixing

```python
member_count = 50

# print("Members: " + member_count)  # TypeError
print("Members:", member_count)
print(f"Members: {member_count}")
```

# Quick Reference

```python
integer_value = 50
float_value = 4.8
complex_value = 2 + 3j
boolean_value = True
string_value = "OSDC"
list_value = ["HTML", "CSS", "Python"]
tuple_value = ("OSDC", "JIIT, Noida")
set_value = {"HTML", "CSS", "Python"}
dictionary_value = {"name": "OSDC"}
range_value = range(5)
empty_value = None
```

| Type | Ordered | Mutable | Allows duplicates |
|---|---:|---:|---:|
| `str` | Yes | No | Yes |
| `list` | Yes | Yes | Yes |
| `tuple` | Yes | No | Yes |
| `set` | No | Yes | No |
| `dict` | Yes | Yes | Keys: No |
| `range` | Yes | No | Yes |
