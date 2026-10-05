---
course: python
slug: loops
title: Python Loops
description: "Learn for loops, while loops, range, break, continue, and repetition in Python."
---

Loops in python are similar to loop in C, except python does not have a do while loop, but we can do a makeshift do while loop in python too.

## For Loop:

A loop which runs "FOR" a specific number of repetitions or iterations.

```
for (int i = 0; i < 10; i++) {
 printf("%d\n", i);
}
```
Or you can write it like this,

```
for i in range(3):
    print(i)
```

`range()` can take up to three values: `range(start, stop, step)`. The `stop` value is never included, so `range(3)` gives 0, 1, 2. If you leave out `start` it begins at 0, and if you leave out `step` it goes up by 1.

```
for i in range(1, 6):        # 1 2 3 4 5
    print(i)

for i in range(0, 10, 2):    # 0 2 4 6 8
    print(i)

for i in range(5, 0, -1):    # 5 4 3 2 1 (counting down)
    print(i)
```

A big difference from C is that Python's `for` loop can go through the items of a collection directly, like a list or a string, without needing an index.

```
fruits = ["apple", "banana", "mango"]
for fruit in fruits:
    print(fruit)

for letter in "Python":
    print(letter)
```

If you need both the position and the item, use `enumerate()`:

```
for index, fruit in enumerate(fruits):
    print(index, fruit)
```

### Nested Loops

A loop inside another loop. The inner loop runs fully for every single run of the outer loop. A classic example is a multiplication table:

```
for i in range(1, 4):
    for j in range(1, 6):
        print(i * j, end="\t")
    print()
```

Output:

```
1	2	3	4	5
2	4	6	8	10
3	6	9	12	15
```

## While Loop:
 A loop which runs "WHILE" a certain condition is true, the number of iterations/repetitions aren't fixed, only the boundary condition.

```
i = 1
while i <= 3:
    print(i)
    i += 1

```
Now here we change the value of i same as in C.

Be careful to always change something inside the loop that will eventually make the condition false. If you forget the `i += 1` above, the condition `i <= 3` stays true forever and you get an **infinite loop**. If that happens, press `Ctrl + C` in the terminal to stop it.

While loops are best when you don't know beforehand how many times something needs to repeat. For example, asking the user for a password until they get it right:

```
password = ""
while password != "python123":
    password = input("Enter the password: ")
print("Access granted")
```

Another example, a simple countdown:

```
count = 5
while count > 0:
    print(count)
    count -= 1
print("Liftoff!")
```

## Do While Loop

A loop to "DO (something) WHILE" a certain condition is true, again the number of iterations/repetitions are not fixed. The difference is the fact that a 'Do While' loop runs the code first then checks the conditions and repeat. Which is the opposite of the 'While' loop.
You can say it assumes that the first/base condition is true by default. Since there is no built in Do While loop in python, we make it using While itself.

```
while True:
    # code
    if condition:
        break

```

Here `while True` makes the loop run endlessly, so the code inside always runs at least once, and the `break` at the end is what stops it when the condition is met. For comparison, this is how it looks in C:

```
do {
    // code
} while (condition);
```

A practical example is a menu that must be shown at least once, and keeps coming back until the user chooses to exit:

```
while True:
    print("1. Say Hello")
    print("2. Say Bye")
    print("3. Exit")
    choice = int(input("Enter your choice: "))

    if choice == 1:
        print("Hello!")
    elif choice == 2:
        print("Bye!")
    elif choice == 3:
        print("Exiting...")
        break
```

## Loop Control: break, continue and pass

These three keywords change how a loop behaves.

- `break` stops the loop completely and moves on to the code after it.
- `continue` skips the rest of the current iteration and jumps to the next one.
- `pass` does nothing, it is just a placeholder where Python expects some code.

```
for i in range(1, 8):
    if i == 3:
        continue     # skip 3
    if i == 6:
        break        # stop at 6
    print(i)         # prints 1 2 4 5
```

## The else Block on Loops

Python has a unique feature that C doesn't: a loop can have an `else` block, which runs only if the loop finished normally, meaning it was **not** stopped by a `break`.

```
for n in range(2, 10):
    if n % 5 == 0:
        print("Found a multiple of 5:", n)
        break
else:
    print("No multiple of 5 found")
```

## Quick Summary

| Loop | Use it when | Python keyword |
|------|-------------|----------------|
| For | You know how many times, or are going through a collection | `for` |
| While | You only know the condition to keep going | `while` |
| Do While | The code must run at least once first | `while True` + `break` |
