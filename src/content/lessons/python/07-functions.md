---
course: python
slug: functions
title: Functions
description: "Learn how to create reusable functions with parameters, return values, and local scope."
---

A function is a reusable block of code that performs a specific task. Functions help divide a large program into smaller, clearer, and reusable parts.

Python provides built-in functions such as `print()`, `input()`, `len()`, and `sum()`. You can also create your own functions. These are called **user-defined functions**.

# Why Use Functions?

Functions help you:

- Reuse code instead of writing it repeatedly
- Divide a large problem into smaller parts
- Give meaningful names to operations
- Make programs easier to read and test
- Reduce duplication and maintenance effort

For example, instead of writing the same welcome message multiple times:

```python
print("Welcome to OSDC.")
print("Welcome to OSDC.")
print("Welcome to OSDC.")
```

Create a function and call it whenever the message is needed:

```python
def show_welcome_message():
    print("Welcome to OSDC.")


show_welcome_message()
show_welcome_message()
show_welcome_message()
```

# Defining a Function

Use the `def` keyword to define a function.

```python
def function_name():
    # statements belonging to the function
    pass
```

A function definition contains:

1. The `def` keyword
2. The function name
3. Parentheses `()`
4. A colon `:`
5. An indented function body

```python
def show_club_name():
    print("OSDC")
```

Defining a function does not execute its body. The function runs only when it is called.

# Calling a Function

Call a function by writing its name followed by parentheses.

```python
def show_club_name():
    print("OSDC")


show_club_name()
```

A function can be called multiple times:

```python
def show_workshop_topic():
    print("Python and FastAPI")


show_workshop_topic()
show_workshop_topic()
```

## Function Execution Order

Python executes a program from top to bottom. The function must be defined before it is called.

```python
def display_message():
    print("OSDC workshop")


print("Before the call")
display_message()
print("After the call")
```

Output:

```text
Before the call
OSDC workshop
After the call
```

# Parameters and Arguments

A **parameter** is a variable listed in a function definition. An **argument** is the value passed to the function when it is called.

```python
def greet_club(club_name):  # club_name is a parameter
    print(f"Welcome to {club_name}.")


greet_club("OSDC")  # "OSDC" is an argument
```

A function can have multiple parameters:

```python
def describe_workshop(workshop_name, topic):
    print(f"Workshop: {workshop_name}")
    print(f"Topic: {topic}")


describe_workshop("Full Stack Workshop", "HTML, CSS, JavaScript, Python, and FastAPI")
```

Arguments are matched with parameters according to their position when positional arguments are used.

```python
def show_workshop(name, duration):
    print(f"{name} lasts {duration} days.")


show_workshop("Python Workshop", 2)
```

# Positional Arguments

With positional arguments, values are assigned to parameters in the order in which they are passed.

```python
def register_workshop(workshop_name, seat_count):
    print(f"Workshop: {workshop_name}")
    print(f"Seats: {seat_count}")


register_workshop("FastAPI Workshop", 40)
```

The following call reverses the values and produces incorrect meaning, even though Python accepts it:

```python
# register_workshop(40, "FastAPI Workshop")
```

Use the correct order or keyword arguments when the meaning needs to be especially clear.

# Keyword Arguments

Keyword arguments are passed using parameter names.

```python
def register_workshop(workshop_name, seat_count):
    print(f"Workshop: {workshop_name}")
    print(f"Seats: {seat_count}")


register_workshop(
    seat_count=40,
    workshop_name="FastAPI Workshop"
)
```

Keyword arguments can be passed in a different order from the function definition.

Positional arguments must come before keyword arguments:

```python
def describe_workshop(name, topic, duration):
    print(f"{name}: {topic} ({duration} days)")


describe_workshop("Python Workshop", topic="Python", duration=2)
```

# Returning Values

The `return` statement sends a value back to the code that called the function.

```python
def add_numbers(first_number, second_number):
    return first_number + second_number


result = add_numbers(10, 20)
print(result)
```

A returned value can be stored, printed, or used in another expression:

```python
def calculate_total_attendance(first_day, second_day):
    return first_day + second_day


total = calculate_total_attendance(40, 35)
average = total / 2

print("Total attendance:", total)
print("Average attendance:", average)
```

## `return` Stops a Function

When Python reaches `return`, the function stops immediately.

```python
def check_seats(seat_count):
    if seat_count <= 0:
        return "Workshop full"

    return "Seats available"


print(check_seats(10))
print(check_seats(0))
```

Code written after an unconditional `return` is unreachable and will not run.

## Returning Multiple Values

A function can return multiple values. Python returns them as a tuple.

```python
def get_workshop_details():
    return "Python Workshop", 2


name, duration = get_workshop_details()

print(name)
print(duration)
```

A function can also return a dictionary when named fields make the result clearer:

```python
def get_club_details():
    return {
        "name": "OSDC",
        "institution": "JIIT, Noida",
        "focus": "Open Source Development"
    }


club = get_club_details()
print(club["name"])
```

# Functions Without `return`

If a function does not explicitly return a value, it returns `None`.

```python
def show_message():
    print("Welcome to OSDC.")


result = show_message()
print(result)
# None
```

Printing a value and returning a value are different actions:

```python
def print_member_count(member_count):
    print(member_count)


def get_member_count(member_count):
    return member_count


printed_value = print_member_count(50)
returned_value = get_member_count(50)

print(printed_value)   # None
print(returned_value)  # 50
```

Use `return` when the calling code needs to work with the result.

# Default Parameters

A default parameter has a value that is used when the caller does not provide an argument.

```python
def greet_member(member_name="OSDC member"):
    print(f"Welcome, {member_name}.")


greet_member("Workshop participant")
greet_member()
```

Parameters with defaults must come after parameters without defaults.

```python
def create_workshop(workshop_name, duration=2):
    print(f"{workshop_name} lasts {duration} days.")


create_workshop("Python Workshop")
create_workshop("FastAPI Workshop", 3)
```

This definition is invalid because a required parameter follows a default parameter:

```python
# def create_workshop(duration=2, workshop_name):
#     pass
```

## Avoid Mutable Default Values

Do not use a list or dictionary as a default parameter when the function will modify it.

Avoid this:

```python
def add_topic(topic, topics=[]):
    topics.append(topic)
    return topics
```

Use `None` and create a new list inside the function:

```python
def add_topic(topic, topics=None):
    if topics is None:
        topics = []

    topics.append(topic)
    return topics


print(add_topic("Python"))
print(add_topic("FastAPI"))
```

# Local and Global Scope

The scope of a variable is the part of the program where that variable can be accessed.

A variable created inside a function is local to that function.

```python
def show_workshop():
    workshop_name = "Python Workshop"
    print(workshop_name)


show_workshop()
# print(workshop_name)  # NameError
```

A variable created outside a function is in the global scope.

```python
club_name = "OSDC"


def show_club_name():
    print(club_name)


show_club_name()
```

A function can read a global variable, but it is usually better to pass data as a parameter.

```python
def show_club_name(club_name):
    print(club_name)


show_club_name("OSDC")
```

## Local Variables with the Same Name

A local variable can have the same name as a global variable. The local variable is used inside the function.

```python
club_name = "OSDC"


def display_club():
    club_name = "OSDC Development Team"
    print(club_name)


display_club()
print(club_name)
```

Avoid relying heavily on global variables because they make functions harder to test and reuse.

# Functions with Mutable Arguments

Lists and dictionaries are mutable. If a function modifies a mutable object passed to it, the change is visible outside the function.

```python
def add_workshop(workshops, workshop_name):
    workshops.append(workshop_name)


workshops = ["HTML and CSS"]
add_workshop(workshops, "Python and FastAPI")

print(workshops)
```

If you do not want the original list to change, pass a copy or create one inside the function.

```python
def add_workshop_without_changing_original(workshops, workshop_name):
    updated_workshops = workshops.copy()
    updated_workshops.append(workshop_name)
    return updated_workshops


workshops = ["HTML and CSS"]
updated_workshops = add_workshop_without_changing_original(
    workshops,
    "Python and FastAPI"
)

print(workshops)
print(updated_workshops)
```

# Arbitrary Positional Arguments: `*args`

Use `*args` when a function should accept any number of positional arguments. Inside the function, `args` is a tuple.

```python
def show_topics(*topics):
    for topic in topics:
        print(topic)


show_topics("HTML", "CSS", "JavaScript", "Python", "FastAPI")
```

The name `args` is a convention. The important part is the `*`.

```python
def calculate_total(*attendance_values):
    return sum(attendance_values)


print(calculate_total(40, 35, 50))
```

Regular parameters can come before `*args`:

```python
def show_workshop_topics(workshop_name, *topics):
    print(f"Workshop: {workshop_name}")

    for topic in topics:
        print(f"- {topic}")


show_workshop_topics(
    "Full Stack Workshop",
    "HTML",
    "CSS",
    "JavaScript",
    "Python",
    "FastAPI"
)
```

# Arbitrary Keyword Arguments: `**kwargs`

Use `**kwargs` when a function should accept any number of keyword arguments. Inside the function, `kwargs` is a dictionary.

```python
def show_club_details(**details):
    for key, value in details.items():
        print(f"{key}: {value}")


show_club_details(
    name="OSDC",
    institution="JIIT, Noida",
    focus="Open Source Development"
)
```

The name `kwargs` is a convention. The important part is the `**`.

Regular parameters and `**kwargs` can be combined:

```python
def create_workshop(workshop_name, **details):
    print(f"Workshop: {workshop_name}")

    for key, value in details.items():
        print(f"{key}: {value}")


create_workshop(
    "Python Workshop",
    duration=2,
    level="Beginner",
    organiser="OSDC"
)
```

# Keyword-Only Parameters

Parameters after `*` must be passed by keyword.

```python
def register_workshop(workshop_name, *, seat_count, is_online):
    print(workshop_name)
    print(seat_count)
    print(is_online)


register_workshop(
    "FastAPI Workshop",
    seat_count=40,
    is_online=False
)
```

This prevents unclear positional calls such as `register_workshop("FastAPI Workshop", 40, False)`.

# Positional-Only Parameters

Parameters before `/` can be passed only positionally. This syntax is available in Python 3.8 and later.

```python
def calculate_total(first_value, second_value, /):
    return first_value + second_value


print(calculate_total(10, 20))
```

This call is invalid because the parameters are positional-only:

```python
# calculate_total(first_value=10, second_value=20)
```

Most beginner functions do not need positional-only parameters, but they are useful when designing strict public APIs.

# Function Annotations

Type hints can document the expected types of parameters and the return value.

```python
def calculate_total(first_value: int, second_value: int) -> int:
    return first_value + second_value


print(calculate_total(10, 20))
```

Python does not enforce these annotations automatically. Type checkers and code editors can use them to identify possible mistakes.

With collections:

```python
def get_topics() -> list[str]:
    return ["HTML", "CSS", "JavaScript", "Python", "FastAPI"]


def count_workshops(workshops: list[str]) -> int:
    return len(workshops)


workshops = get_topics()
print(count_workshops(workshops))
```

# Docstrings

A docstring describes what a function does. It is written as the first statement inside the function body.

```python
def calculate_average(first_value, second_value):
    """Return the average of two numeric values."""
    return (first_value + second_value) / 2


print(calculate_average(40, 50))
```

You can access a function's docstring using `__doc__`:

```python
print(calculate_average.__doc__)
```

A useful docstring explains the function's purpose, parameters, and return value when the function is more complex.

```python
def get_available_seats(total_seats, registered_members):
    """Return the number of seats that are still available."""
    return total_seats - registered_members
```

# Functions Calling Other Functions

Functions can call other functions to divide a task into smaller steps.

```python
def calculate_available_seats(total_seats, registered_members):
    return total_seats - registered_members


def show_registration_status(total_seats, registered_members):
    available_seats = calculate_available_seats(
        total_seats,
        registered_members
    )

    if available_seats > 0:
        print(f"{available_seats} seats are available.")
    else:
        print("The workshop is full.")


show_registration_status(50, 42)
```

This keeps each function focused on one responsibility.

# Functions with Input

Input can be collected inside a function, and the result can be returned to the caller.

```python
def get_member_count():
    while True:
        try:
            member_count = int(input("Enter the OSDC member count: "))

            if member_count >= 0:
                return member_count

            print("Member count cannot be negative.")
        except ValueError:
            print("Please enter a whole number.")


member_count = get_member_count()
print(f"OSDC member count: {member_count}")
```

A function can also receive input as a parameter. This makes the function easier to test because it does not depend on interactive input.

```python
def describe_workshop(workshop_name, seat_count):
    return f"{workshop_name} has {seat_count} seats."


workshop_name = input("Workshop name: ").strip()
seat_count = int(input("Number of seats: "))

print(describe_workshop(workshop_name, seat_count))
```

# Recursive Functions

A recursive function calls itself. Every recursive function needs:

1. A base case that stops the recursion
2. A recursive case that moves toward the base case

```python
def countdown(number):
    if number == 0:
        print("Start!")
        return

    print(number)
    countdown(number - 1)


countdown(3)
```

Output:

```text
3
2
1
Start!
```

Without a base case, the function would continue calling itself until Python raises a `RecursionError`.

A recursive factorial function:

```python
def factorial(number):
    if number == 0 or number == 1:
        return 1

    return number * factorial(number - 1)


print(factorial(5))
```

Loops are often simpler and more efficient for basic repetition. Recursion is useful when a problem naturally contains smaller versions of itself, such as traversing nested structures.

# Lambda Functions

A lambda is a small anonymous function containing one expression.

```python
square = lambda number: number ** 2

print(square(5))
```

A regular function is usually clearer when the operation is reused or contains multiple statements:

```python
def square(number):
    return number ** 2
```

Lambda functions are commonly used with functions such as `sorted()`:

```python
workshops = [
    {"name": "Python", "attendance": 45},
    {"name": "FastAPI", "attendance": 30},
    {"name": "JavaScript", "attendance": 50}
]

sorted_workshops = sorted(
    workshops,
    key=lambda workshop: workshop["attendance"]
)

print(sorted_workshops)
```

# Functions as Values

Functions can be stored in variables and passed to other functions.

```python
def show_python():
    print("Python workshop")


selected_action = show_python
selected_action()
```

A function that accepts another function is called a higher-order function.

```python
def run_action(action):
    action()


def show_osdc_message():
    print("Welcome to OSDC.")


run_action(show_osdc_message)
```

# A Function-Based Menu

Functions are useful for organizing a menu-driven program.

```python
def show_workshops():
    print("Available workshops:")
    print("- HTML and CSS")
    print("- JavaScript")
    print("- Python")
    print("- FastAPI")


def show_club_information():
    print("OSDC")
    print("Open Source Developers Community")
    print("JIIT, Noida")


def show_menu():
    print("\nOSDC Menu")
    print("1. View workshops")
    print("2. View club information")
    print("3. Exit")


while True:
    show_menu()
    choice = input("Enter your choice: ").strip()

    if choice == "1":
        show_workshops()
    elif choice == "2":
        show_club_information()
    elif choice == "3":
        print("Goodbye.")
        break
    else:
        print("Invalid choice.")
```

# Common Mistakes

## Defining but Not Calling a Function

```python
def show_message():
    print("Welcome to OSDC.")

# The function does not run until it is called.
show_message()
```

## Forgetting to Return a Value

```python
def add_numbers(first_number, second_number):
    first_number + second_number  # The result is not returned


result = add_numbers(10, 20)
print(result)  # None
```

Correct version:

```python
def add_numbers(first_number, second_number):
    return first_number + second_number
```

## Using the Wrong Number of Arguments

```python
def show_workshop(name, duration):
    print(f"{name}: {duration} days")


# show_workshop("Python Workshop")  # TypeError
show_workshop("Python Workshop", 2)
```

## Mutable Default Arguments

Use `None` instead of a list or dictionary as a default value when the function modifies it.

```python
def add_topic(topic, topics=None):
    if topics is None:
        topics = []

    topics.append(topic)
    return topics
```

## Overusing Global Variables

Pass data as parameters and return results instead of changing global variables from inside functions.

```python
def add_member(member_count):
    return member_count + 1


member_count = 50
member_count = add_member(member_count)
print(member_count)
```

# Quick Reference

```python
def function_name(parameter):
    return parameter


result = function_name(argument)
```

```python
def function_name(parameter, optional_parameter=default_value):
    return parameter
```

```python
def function_name(*args, **kwargs):
    pass
```

| Concept | Meaning |
|---|---|
| `def` | Starts a function definition |
| Parameter | Variable in a function definition |
| Argument | Value passed during a function call |
| `return` | Sends a value back to the caller |
| Default parameter | Value used when an argument is omitted |
| `*args` | Accepts extra positional arguments |
| `**kwargs` | Accepts extra keyword arguments |
| Docstring | Description written inside a function |
| Local variable | Variable available inside its function |
| Global variable | Variable defined outside functions |
