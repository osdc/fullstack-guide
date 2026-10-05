---
course: python
slug: input
title: Python Input
description: "Learn how to accept user input, convert values, and build interactive Python programs."
---

Programs become interactive when they can receive information from a user. Python uses the built-in `input()` function to read input from the keyboard.

## Basic Syntax

```python
input()
```

The program pauses until the user enters a value and presses **Enter**.

```python
input()
print("Input received.")
```

The value entered by the user is returned by `input()`. To use that value later, store it in a variable.

```python
club_name = input()

print("You entered:", club_name)
```

## Adding a Prompt

A prompt tells the user what to enter. Pass the prompt text inside the parentheses.

```python
club_name = input("Enter the club name: ")

print("Club:", club_name)
```

The prompt does not automatically add a new line. The user's input appears on the same line.

```python
workshop_name = input("Enter the workshop name: ")
print("Workshop:", workshop_name)
```

Example interaction:

```text
Enter the workshop name: Python and FastAPI
Workshop: Python and FastAPI
```

## `input()` Always Returns a String

Even if the user enters a number, `input()` returns it as a string.

```python
member_count = input("Enter the number of members: ")

print(member_count)
print(type(member_count))
```

If the user enters `50`, the output is:

```text
50
<class 'str'>
```

This means mathematical operations cannot be performed on the input until it is converted.

```python
member_count = input("Enter the number of members: ")

# print(member_count + 10)  # TypeError
```

## Converting Input to a Number

Use `int()` to convert input to an integer.

```python
member_count = int(input("Enter the number of members: "))

print(member_count + 10)
print(type(member_count))
```

Use `float()` when the input may contain a decimal value.

```python
event_rating = float(input("Enter the event rating: "))

print("Rating:", event_rating)
print(type(event_rating))
```

The conversion happens from the inside out:

```python
member_count = int(input("Enter the number of members: "))
```

1. `input()` reads text from the user.
2. `int()` converts that text to an integer.
3. The converted value is assigned to `member_count`.

## Reading Different Types of Values

```python
club_name = input("Enter the club name: ")
member_count = int(input("Enter the member count: "))
event_rating = float(input("Enter the event rating: "))
registration_open = input("Are registrations open? ") == "yes"

print(club_name)
print(member_count)
print(event_rating)
print(registration_open)
```

Python does not have a separate input function for every type. Read the value with `input()` and convert it when required.

## Converting Boolean Input

Using `bool()` directly on user input can give unexpected results.

```python
answer = input("Enter yes or no: ")
print(bool(answer))
```

Both `"yes"` and `"no"` are non-empty strings, so both are considered `True`.

Instead, compare the input with an expected value.

```python
answer = input("Are OSDC registrations open? ").strip().lower()
registration_open = answer == "yes"

print(registration_open)
```

You can accept multiple forms of an answer:

```python
answer = input("Are registrations open? [yes/no]: ").strip().lower()

if answer in ("yes", "y"):
    print("Registrations are open.")
elif answer in ("no", "n"):
    print("Registrations are closed.")
else:
    print("Please enter yes or no.")
```

## Removing Extra Whitespace

Users may accidentally enter spaces before or after their input. Use `.strip()` to remove them.

```python
club_name = input("Enter the club name: ").strip()

print("Club:", club_name)
```

Useful string methods can be combined with `input()`:

```python
technology = input("Enter a technology: ").strip().lower()

print("Selected technology:", technology)
```

## Handling Invalid Input

Converting invalid text with `int()` or `float()` raises a `ValueError`.

```python
member_count = int(input("Enter the number of members: "))
```

If the user enters `many`, the program stops with an error. Use `try` and `except` to handle invalid input.

```python
try:
    member_count = int(input("Enter the number of members: "))
    print("Member count:", member_count)
except ValueError:
    print("Please enter a whole number.")
```

A complete validation loop keeps asking until the user enters valid input.

```python
while True:
    try:
        member_count = int(input("Enter the number of members: "))

        if member_count < 0:
            print("Member count cannot be negative.")
            continue

        break
    except ValueError:
        print("Please enter a whole number.")

print("Member count:", member_count)
```

## Validating Text Input

A text input can be checked before it is used.

```python
club_name = input("Enter the club name: ").strip()

if club_name == "":
    print("Club name cannot be empty.")
else:
    print("Club:", club_name)
```

The same check can be written using the truth value of a string:

```python
club_name = input("Enter the club name: ").strip()

if not club_name:
    print("Club name cannot be empty.")
else:
    print("Club:", club_name)
```

## Reading Multiple Values

### Separate Inputs

The clearest approach is to ask for each value separately.

```python
workshop_name = input("Workshop name: ").strip()
workshop_day = input("Workshop day: ").strip()

print(f"{workshop_name} will be held on {workshop_day}.")
```

### Multiple Values on One Line

Use `.split()` when several values are entered on one line.

```python
first_name, last_name = input("Enter two words: ").split()

print("First:", first_name)
print("Last:", last_name)
```

For numbers, convert every value after splitting.

```python
attendance = input("Enter attendance for three workshops: ").split()
attendance = [int(value) for value in attendance]

print(attendance)
print("Total attendance:", sum(attendance))
```

The input must contain exactly three values in this example. Otherwise, unpacking raises an error.

```python
# Example input: 40 35 50
attendance = [int(value) for value in input().split()]
print(attendance)
```

### Comma-Separated Input

Use `.split(",")` for comma-separated values.

```python
technologies = input("Enter technologies separated by commas: ").split(",")
technologies = [technology.strip() for technology in technologies]

print(technologies)
```

Example interaction:

```text
Enter technologies separated by commas: HTML, CSS, JavaScript, Python
['HTML', 'CSS', 'JavaScript', 'Python']
```

## Input and Formatted Output

Input values can be displayed using f-strings.

```python
club_name = input("Club name: ").strip()
workshop_name = input("Workshop name: ").strip()

print(f"{club_name} is conducting a {workshop_name}.")
```

With numeric input:

```python
member_count = int(input("Number of members: "))

print(f"OSDC has {member_count} registered members.")
```

## A Complete Example

The following program collects information about an OSDC workshop and validates the numeric values.

```python
workshop_name = input("Workshop name: ").strip()
workshop_topic = input("Workshop topic: ").strip()

while True:
    try:
        seat_count = int(input("Number of seats: "))
        if seat_count <= 0:
            print("The number of seats must be greater than zero.")
            continue
        break
    except ValueError:
        print("Please enter a valid whole number.")

print("\nWorkshop details")
print("---------------")
print(f"Organiser: OSDC")
print(f"Workshop: {workshop_name}")
print(f"Topic: {workshop_topic}")
print(f"Seats: {seat_count}")
```

## Common Mistakes

### Forgetting Type Conversion

```python
first_count = input("First workshop attendance: ")
second_count = input("Second workshop attendance: ")

# This joins strings instead of adding numbers.
print(first_count + second_count)
```

For inputs `40` and `35`, the output is `4035`. Convert both values first:

```python
first_count = int(input("First workshop attendance: "))
second_count = int(input("Second workshop attendance: "))

print(first_count + second_count)
```

### Calling `int()` Before Checking Invalid Input

```python
# This raises ValueError when the input is not a number.
member_count = int(input("Member count: "))
```

Use `try` and `except` when the input may be invalid.

### Expecting `input()` to Return a Boolean

```python
# This does not correctly interpret "no" as False.
registration_open = bool(input("Are registrations open? "))
```

Compare the input with an expected answer instead:

```python
answer = input("Are registrations open? [yes/no]: ").strip().lower()
registration_open = answer == "yes"
```

### Not Removing Whitespace

```python
technology = input("Technology: ")

if technology == "python":
    print("Python selected.")
```

An input such as ` Python ` does not match. Normalize it first:

```python
technology = input("Technology: ").strip().lower()

if technology == "python":
    print("Python selected.")
```

## Input in a Function

Input can be collected inside a function. Returning the value makes the function reusable.

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

A function can also accept input as a parameter instead of reading it itself. This makes it easier to test.

```python
def describe_workshop(workshop_name, seat_count):
    return f"{workshop_name} has {seat_count} seats."


workshop_name = input("Workshop name: ").strip()
seat_count = int(input("Number of seats: "))

print(describe_workshop(workshop_name, seat_count))
```

## Quick Reference

| Task | Example |
|---|---|
| Read text | `value = input("Enter a value: ")` |
| Read an integer | `value = int(input("Enter a number: "))` |
| Read a float | `value = float(input("Enter a rating: "))` |
| Remove surrounding spaces | `value = input().strip()` |
| Convert to lowercase | `value = input().lower()` |
| Read multiple values | `values = input().split()` |
| Read comma-separated values | `values = input().split(",")` |
| Handle invalid numbers | `try` and `except ValueError` |
