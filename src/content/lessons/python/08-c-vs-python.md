---
course: python
slug: c-vs-python
title: C vs Python
description: "Compare C and Python syntax, programming concepts, data types, functions, and common patterns."
---

Both C and Python are very popular, but they are built differently. C is a lower level language that gives you more control and runs very fast, while Python is higher level, shorter to write and easier to read. Below is the same simple code written in both, so the differences are easy to spot.

## Hello World

**C**
```
#include <stdio.h>

int main() {
    printf("Hello World\n");
    return 0;
}
```

**Python**
```
print("Hello World")
```

C needs a header file, a `main` function and a return statement. Python needs just one line.

## Variables

**C**
```
int age = 19;
float price = 49.99;
char grade = 'A';
```

**Python**
```
age = 19
price = 49.99
grade = "A"
```

In C you declare the type. Python works out the type by itself, and it even lets the same variable change type later.

## Taking Input

**C**
```
int age;
printf("Enter your age: ");
scanf("%d", &age);
printf("You are %d years old\n", age);
```

**Python**
```
age = int(input("Enter your age: "))
print("You are", age, "years old")
```

No format specifiers (`%d`, `%f`) and no `&` in Python.

## If / Else

**C**
```
if (marks >= 90) {
    printf("Grade A\n");
} else if (marks >= 60) {
    printf("Grade B\n");
} else {
    printf("Grade C\n");
}
```

**Python**
```
if marks >= 90:
    print("Grade A")
elif marks >= 60:
    print("Grade B")
else:
    print("Grade C")
```

Python uses a colon and indentation instead of brackets and curly braces, and `else if` becomes `elif`.

## For Loop

**C**
```
for (int i = 0; i < 5; i++) {
    printf("%d\n", i);
}
```

**Python**
```
for i in range(5):
    print(i)
```

## While Loop

**C**
```
int i = 1;
while (i <= 3) {
    printf("%d\n", i);
    i++;
}
```

**Python**
```
i = 1
while i <= 3:
    print(i)
    i += 1
```

Python has no `i++`, you write `i += 1` instead.

## Do While Loop

**C**
```
int n;
do {
    printf("Enter a positive number: ");
    scanf("%d", &n);
} while (n <= 0);
```

**Python**
```
while True:
    n = int(input("Enter a positive number: "))
    if n > 0:
        break
```

## Functions

**C**
```
int add(int a, int b) {
    return a + b;
}

int main() {
    printf("%d\n", add(3, 4));
    return 0;
}
```

**Python**
```
def add(a, b):
    return a + b

print(add(3, 4))
```

## Arrays vs Lists

**C**
```
int numbers[5] = {1, 2, 3, 4, 5};
printf("%d\n", numbers[0]);
```

**Python**
```
numbers = [1, 2, 3, 4, 5]
print(numbers[0])
```

A C array has a fixed size and holds only one type. A Python list can grow or shrink and can hold mixed types:

```
numbers.append(6)           # add an item
numbers.remove(2)           # remove an item
mixed = [1, "hello", 3.5]   # different types together
```

## Strings

**C**
```
char name[] = "Aryan";
printf("%s\n", name);
printf("%lu\n", strlen(name));
```

**Python**
```
name = "Aryan"
print(name)
print(len(name))
```

Python strings also come with handy built in tricks:

```
print(name.upper())      # ARYAN
print(name + " Singh")   # joining strings with +
print(name[0])           # A
```

## Swapping Two Numbers

**C**
```
int temp = a;
a = b;
b = temp;
```

**Python**
```
a, b = b, a
```

## Sum of a List of Numbers

**C**
```
int nums[5] = {1, 2, 3, 4, 5};
int sum = 0;
for (int i = 0; i < 5; i++) {
    sum += nums[i];
}
printf("%d\n", sum);
```

**Python**
```
nums = [1, 2, 3, 4, 5]
print(sum(nums))
```

## Side by Side Summary

| Feature | C | Python |
|---------|---|--------|
| Type | Compiled | Interpreted |
| Speed | Very fast | Slower |
| Semicolons | Required | Not needed |
| Code blocks | `{ }` curly braces | Indentation |
| Variable types | Must be declared | Decided automatically |
| Format specifiers | `%d`, `%f`, `%s` | Not needed |
| Do While loop | Built in | Made using `while True` + `break` |
| Arrays | Fixed size, one type | Lists, flexible size and mixed types |
| Memory management | Manual (pointers, `malloc`) | Automatic |
| Libraries | `#include <stdio.h>` | `import math` |
| Best for | Operating systems, embedded systems, speed critical programs | Data science, web, automation, AI, quick scripts |

## So Which One Should You Use?

Neither is "better", they are made for different jobs. Learning C helps you understand how a computer actually works (memory, pointers, types), and learning Python lets you build things quickly. Knowing both makes you a stronger programmer, and the concepts you learn in one carry over easily to the other.
