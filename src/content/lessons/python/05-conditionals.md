---
course: python
slug: conditionals
title: Python If Else and Menu Driven Programs
description: "Learn conditions, comparisons, if/elif/else statements, and how to build menu-driven programs."
---

Programs often need to make decisions. For example, a program may check whether OSDC registrations are open, decide whether a workshop has available seats, or perform an action selected from a menu.

Python uses conditional statements to execute different code based on whether a condition is true or false.

# Conditions

A condition is an expression that evaluates to either `True` or `False`.

```python
member_count = 50

print(member_count > 0)
print(member_count == 50)
print(member_count < 10)
```

Comparison operators:

| Operator | Meaning | Example |
|---|---|---|
| `==` | Equal to | `count == 50` |
| `!=` | Not equal to | `count != 50` |
| `>` | Greater than | `count > 50` |
| `<` | Less than | `count < 50` |
| `>=` | Greater than or equal to | `count >= 50` |
| `<=` | Less than or equal to | `count <= 50` |

Do not confuse `=` and `==`:

```python
member_count = 50          # assignment
print(member_count == 50)  # comparison
```

# The `if` Statement

The `if` statement runs a block of code only when its condition is true.

```python
registration_open = True

if registration_open:
    print("OSDC registrations are open.")
```

Python uses indentation to define the block belonging to an `if` statement. The usual indentation is four spaces, and the colon after the condition is required.

```python
member_count = 50

if member_count > 0:
    print("OSDC has registered members.")
```

## Using Input with `if`

`input()` returns a string, so convert numeric input before comparing it with a number.

```python
seat_count = int(input("Enter the number of available seats: "))

if seat_count > 0:
    print("Seats are available.")
```

For text, normalize input before comparing it:

```python
technology = input("Enter a technology: ").strip().lower()

if technology == "python":
    print("Python is used in the backend workshop.")
```

# The `if-else` Statement

Use `else` when one block should run if the `if` condition is false.

```python
registration_open = False

if registration_open:
    print("You can register for the workshop.")
else:
    print("Registration is currently closed.")
```

Only one of the two blocks runs.

```python
member_count = int(input("Enter the number of OSDC members: "))

if member_count >= 10:
    print("The club has a large member group.")
else:
    print("The club has a small member group.")
```

# The `if-elif-else` Statement

Use `elif`, short for **else if**, when there are multiple possible conditions.

```python
attendance = int(input("Enter workshop attendance: "))

if attendance >= 50:
    print("Excellent attendance.")
elif attendance >= 25:
    print("Good attendance.")
elif attendance > 0:
    print("Some members attended.")
else:
    print("No attendance recorded.")
```

Python checks conditions from top to bottom. Once a condition is true, its block runs and the remaining conditions are skipped.

The order of conditions matters:

```python
score = 85

if score >= 80:
    print("Excellent score.")
elif score >= 40:
    print("Passed.")
else:
    print("Did not pass.")
```

An `else` block is optional. An `if` statement can have multiple `elif` blocks, but only one `else` block.

# Logical Operators

Logical operators combine conditions.

## `and`

`and` is true only when both conditions are true.

```python
member_count = 50
registration_open = True

if member_count > 0 and registration_open:
    print("OSDC can accept registrations.")
```

## `or`

`or` is true when at least one condition is true.

```python
technology = input("Enter HTML, CSS, or Python: ").strip().lower()

if technology == "html" or technology == "css":
    print("This is a frontend technology.")
```

For multiple possible values, membership testing is often clearer:

```python
technology = input("Enter a technology: ").strip().lower()

if technology in ("html", "css", "javascript"):
    print("Frontend technology selected.")
```

## `not`

`not` reverses a Boolean value.

```python
registration_open = False

if not registration_open:
    print("Registrations are closed.")
```

Use parentheses to make complex conditions clear:

```python
member_count = 30
registration_open = True

if (member_count > 0 and registration_open) or member_count == 0:
    print("The registration system can be displayed.")
```

# Truthy and Falsy Values

Python allows many values to be used directly as conditions.

The following values are falsy:

- `False`
- `None`
- `0`
- `0.0`
- `""`
- Empty collections such as `[]`, `{}`, `()`, and `set()`

Most other values are truthy.

```python
club_name = input("Enter the club name: ").strip()

if club_name:
    print(f"Welcome to {club_name}.")
else:
    print("The club name cannot be empty.")
```

# Nested Conditions

An `if` statement can be placed inside another `if` statement.

```python
registration_open = True
seat_count = 20

if registration_open:
    if seat_count > 0:
        print("Registration is available.")
    else:
        print("The workshop is full.")
else:
    print("Registration is closed.")
```

The same logic can often be expressed more simply:

```python
if registration_open and seat_count > 0:
    print("Registration is available.")
else:
    print("Registration is unavailable.")
```

# Conditional Expressions

A conditional expression selects one of two values in a single line.

```python
seat_count = 10
status = "Seats available" if seat_count > 0 else "Workshop full"

print(status)
```

The general syntax is:

```python
value_if_true if condition else value_if_false
```

Use a regular `if-else` statement when the logic contains multiple steps. Conditional expressions are best for short assignments or output values.

# Validating Conditions

Conditions are useful for validating user input.

```python
try:
    seat_count = int(input("Enter the number of workshop seats: "))

    if seat_count <= 0:
        print("The number of seats must be greater than zero.")
    else:
        print(f"The workshop has {seat_count} seats.")
except ValueError:
    print("Please enter a whole number.")
```

A range can be validated using chained comparisons:

```python
rating = float(input("Enter the workshop rating from 0 to 5: "))

if 0 <= rating <= 5:
    print("Valid rating.")
else:
    print("Rating must be between 0 and 5.")
```

# Menu-Driven Programs

A menu-driven program displays a list of choices and performs an action based on the user's selection.

A basic menu has three parts:

1. Display the available options.
2. Read the user's choice.
3. Use conditional logic to perform the selected action.

## Basic Menu

```python
print("OSDC Workshop Menu")
print("1. View workshops")
print("2. View club information")
print("3. Exit")

choice = input("Enter your choice: ").strip()

if choice == "1":
    print("Available workshops: HTML, CSS, JavaScript, Python, FastAPI")
elif choice == "2":
    print("OSDC is the Open Source Developers Community at JIIT, Noida.")
elif choice == "3":
    print("Goodbye.")
else:
    print("Invalid choice.")
```

The menu choice is read as a string, so compare it with values such as `"1"` and `"2"`.

## Repeating a Menu

A `while` loop keeps the menu available until the user chooses to exit.

```python
while True:
    print("\nOSDC Workshop Menu")
    print("1. View workshops")
    print("2. Register for a workshop")
    print("3. View club information")
    print("4. Exit")

    choice = input("Enter your choice: ").strip()

    if choice == "1":
        print("Available workshops: HTML, CSS, JavaScript, Python, FastAPI")
    elif choice == "2":
        print("Workshop registration selected.")
    elif choice == "3":
        print("OSDC is the Open Source Developers Community at JIIT, Noida.")
    elif choice == "4":
        print("Thank you for using the OSDC menu.")
        break
    else:
        print("Invalid choice. Please select an option from 1 to 4.")
```

`break` immediately stops the loop when the user selects the exit option.

## Menu with Functions

Functions keep each menu action separate and make the program easier to maintain.

```python
def show_workshops():
    print("Available workshops:")
    print("- HTML and CSS")
    print("- JavaScript")
    print("- Python and FastAPI")


def show_club_information():
    print("OSDC: Open Source Developers Community")
    print("Institution: JIIT, Noida")


def show_menu():
    print("\nOSDC Workshop Menu")
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

## Menu-Driven Calculator

```python
print("Calculator Menu")
print("1. Add")
print("2. Subtract")
print("3. Multiply")
print("4. Divide")

choice = input("Choose an operation: ").strip()

if choice in ("1", "2", "3", "4"):
    first_number = float(input("Enter the first number: "))
    second_number = float(input("Enter the second number: "))

    if choice == "1":
        print("Result:", first_number + second_number)
    elif choice == "2":
        print("Result:", first_number - second_number)
    elif choice == "3":
        print("Result:", first_number * second_number)
    elif choice == "4":
        if second_number == 0:
            print("A number cannot be divided by zero.")
        else:
            print("Result:", first_number / second_number)
else:
    print("Invalid operation.")
```

The outer condition checks whether the menu choice is valid. The inner condition prevents division by zero.

## Complete OSDC Workshop Menu

```python
def show_workshops():
    print("\nAvailable workshops")
    print("- HTML and CSS")
    print("- JavaScript")
    print("- Python")
    print("- FastAPI")


def register_for_workshop():
    workshop = input("Enter the workshop name: ").strip()

    if not workshop:
        print("Workshop name cannot be empty.")
    else:
        print(f"Registration request received for {workshop}.")


def show_club_information():
    print("\nOSDC")
    print("Open Source Developers Community")
    print("JIIT, Noida")


while True:
    print("\nOSDC Full Stack Workshop")
    print("1. View workshops")
    print("2. Register for a workshop")
    print("3. View club information")
    print("4. Exit")

    choice = input("Enter your choice: ").strip()

    if choice == "1":
        show_workshops()
    elif choice == "2":
        register_for_workshop()
    elif choice == "3":
        show_club_information()
    elif choice == "4":
        print("Thank you for attending the OSDC workshop.")
        break
    else:
        print("Invalid choice. Please select 1, 2, 3, or 4.")
```

# Menu Choice Validation

A menu should reject invalid choices instead of crashing or performing an unintended action.

```python
while True:
    choice = input("Choose 1, 2, or 3: ").strip()

    if choice in ("1", "2", "3"):
        break

    print("Invalid choice. Please try again.")

print(f"You selected option {choice}.")
```

For a numeric menu, validate the conversion as well:

```python
while True:
    try:
        choice = int(input("Choose 1, 2, or 3: "))

        if choice in (1, 2, 3):
            break

        print("Please choose a number from 1 to 3.")
    except ValueError:
        print("Please enter a number.")

print(f"You selected option {choice}.")
```

# `match-case` for Menus

Python 3.10 and later provide `match-case`, which is useful when one value must be compared with several fixed patterns.

```python
choice = input("Enter 1, 2, or 3: ").strip()

match choice:
    case "1":
        print("View workshops selected.")
    case "2":
        print("View club information selected.")
    case "3":
        print("Exit selected.")
    case _:
        print("Invalid choice.")
```

The underscore `_` is the default case. It matches anything that did not match an earlier case.

Use `if-elif-else` when conditions involve ranges or complex expressions. Use `match-case` when comparing one value with several known patterns.

# Common Mistakes

## Using Assignment Instead of Comparison

```python
choice = input("Enter a choice: ")

# if choice = "1":  # SyntaxError
if choice == "1":
    print("Option 1 selected.")
```

## Forgetting the Colon

```python
# if choice == "1"
#     print("Option 1 selected.")

if choice == "1":
    print("Option 1 selected.")
```

## Comparing Input with an Integer Without Conversion

```python
choice = input("Enter 1 or 2: ")

if choice == 1:
    print("Option 1 selected.")
```

The comparison fails because `choice` is a string. Either compare with `"1"` or convert the input:

```python
choice = int(input("Enter 1 or 2: "))

if choice == 1:
    print("Option 1 selected.")
```

## Forgetting to Handle Invalid Choices

Every menu should have an `else` branch or another way to report an invalid choice.

```python
choice = input("Enter a menu choice: ").strip()

if choice == "1":
    print("Option 1 selected.")
elif choice == "2":
    print("Option 2 selected.")
else:
    print("Invalid choice.")
```

## Forgetting to Exit a Menu Loop

Always provide a clear exit condition when using `while True` for a menu.

```python
while True:
    choice = input("Enter 1 to exit: ").strip()

    if choice == "1":
        print("Exiting.")
        break
```

# Quick Reference

```python
if condition:
    pass
```

```python
if condition:
    pass
else:
    pass
```

```python
if first_condition:
    pass
elif second_condition:
    pass
else:
    pass
```

```python
if condition_a and condition_b:
    pass
```

```python
if condition_a or condition_b:
    pass
```

```python
if not condition:
    pass
```

```python
while True:
    choice = input("Choose an option: ")

    if choice == "1":
        pass
    elif choice == "2":
        pass
    elif choice == "3":
        break
    else:
        print("Invalid choice.")
```
