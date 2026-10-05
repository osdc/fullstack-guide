---
course: python
slug: intro
title: Introduction to Python
description: "Learn Python fundamentals, syntax, variables, and how to write your first Python programs."
---

Python is a programming language used to write programs, give computer commands and perform tasks and operations. It is relatively simpler and more forgiving than C in syntax. For example it doesn't require semicolons at the end of each command in C, and doesn't need data specifiers like %d or %f etc.

`
print("Hello World")
`

Python is an **interpreted** language, meaning the code is read and run line by line by the Python interpreter, instead of being compiled into a separate executable file first like in C. This makes it quicker to write and test, since you can change a line and run it again right away without a compile step. The trade off is that Python is generally slower than C when it comes to raw speed, but for most everyday tasks you will never notice.

## Variables and Data Types

In C you have to tell the computer what type a variable is before using it (`int`, `float`, `char`). Python figures this out on its own, which is called **dynamic typing**. You just give the variable a name and a value.

```
age = 19            # int
price = 49.99       # float
name = "Aryan"      # string (str)
is_student = True   # boolean (bool)
```

You can check what type a variable is with `type()`:

```
print(type(age))    # <class 'int'>
print(type(name))   # <class 'str'>
```

## Indentation

This is the biggest difference you will notice from C. Python doesn't use curly braces `{ }` to group code, it uses **indentation** (spaces at the start of the line). Everything indented under a line like `if`, `for` or `while` belongs to it.

```
if age >= 18:
    print("Adult")
    print("Can vote")
print("This line runs no matter what")
```

If the indentation is wrong, Python gives an error, so it is a good habit to always use 4 spaces.

## Comments

Comments are notes in the code that Python ignores. A single line comment starts with `#`, and for longer ones you can use triple quotes.

```
# This is a single line comment

"""
This is a
multi line comment
"""
```

## Taking Input and Showing Output

`print()` shows output and `input()` takes whatever the user types. Note that `input()` always gives back a string, so if you want a number you have to convert it.

```
name = input("Enter your name: ")
age = int(input("Enter your age: "))
print("Hello", name, "you are", age, "years old")
```

Compare this to C, where you would need `scanf("%d", &age);` and format specifiers. Python does it in one line.

## Basic Operators

Most operators work the same as in C, with a few extra ones.

```
print(10 + 3)    # 13   addition
print(10 - 3)    # 7    subtraction
print(10 * 3)    # 30   multiplication
print(10 / 3)    # 3.33 division (always gives a float)
print(10 // 3)   # 3    floor division
print(10 % 3)    # 1    remainder
print(10 ** 3)   # 1000 power
```

## Decision Making

Conditions use `if`, `elif` (short for else if) and `else`.

```
marks = 72

if marks >= 90:
    print("Grade A")
elif marks >= 60:
    print("Grade B")
else:
    print("Grade C")
```

## Running a Python File

Save your code in a file ending with `.py` (for example `hello.py`) and run it from the terminal:

```
python hello.py
```

You can also type `python` alone in the terminal to open an interactive shell, where each line you type runs immediately. This is great for quickly testing small things.
