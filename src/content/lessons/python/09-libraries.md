---
course: python
slug: python-libraries
title: Libraries
description: "Learn how to import and use Python libraries, modules, and packages in your programs."
---

A library is a collection of ready made code that someone else already wrote, which you can use in your own program instead of writing everything from scratch. In C you do this with `#include <stdio.h>`, in python we use the `import` keyword. Python is famous for its huge number of libraries, which is a big reason why it is so popular.

A few words you will hear a lot:

- **Module:** a single python file (`.py`) containing functions, variables and so on.
- **Library / Package:** a bigger collection of modules bundled together.
- **Standard library:** the libraries that come built in with Python, no installation needed (like `math`, `random`, `datetime`).
- **Third party library:** libraries made by other people which you have to install first (like `numpy`, `matplotlib`, `pandas`).

## Installing a Library

Third party libraries are installed from the terminal using `pip`, python's package manager. You only need to do this once.

```
pip install numpy
pip install matplotlib
pip install pandas
```

## The import Keyword

The simplest way, imports the whole library, and you use its functions with a dot.

```
import math

print(math.sqrt(16))    # 4.0
print(math.pi)          # 3.141592653589793
```

## import ... as

`as` gives the library a shorter nickname (an **alias**), so you don't have to type the long name every time. Some aliases are so common that everyone uses them.

```
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd

print(np.array([1, 2, 3]))
```

## from ... import

If you only need one or two things from a library, you can import just those. Then you use them directly, without the library name in front.

```
from math import sqrt, pi

print(sqrt(25))    # 5.0
print(pi)          # 3.141592653589793
```

You can also combine it with `as`:

```
from math import factorial as fact

print(fact(5))     # 120
```

## from ... import * 

The `*` (star) means "import everything" from that library, again usable without the library name.

```
from math import *

print(sqrt(49))
print(sin(0))
print(pi)
```

This looks convenient but it is generally **not recommended** in bigger programs. Since you don't know exactly what got imported, it can overwrite your own variables or functions that have the same name, and it makes the code harder to read because you can't tell where a function came from.

## Summary of Import Styles

| Style | Example | How you use it |
|-------|---------|----------------|
| import | `import math` | `math.sqrt(4)` |
| import as | `import numpy as np` | `np.array([1, 2])` |
| from import | `from math import sqrt` | `sqrt(4)` |
| from import as | `from math import sqrt as sq` | `sq(4)` |
| from import * | `from math import *` | `sqrt(4)` |

## Famous Libraries

### math (built in)

Mathematical functions and constants.

```
import math

print(math.ceil(4.2))     # 5
print(math.floor(4.8))    # 4
print(math.pow(2, 3))     # 8.0
print(math.factorial(5))  # 120
```

### random (built in)

For generating random numbers and picking random things, useful in games and simulations.

```
import random

print(random.randint(1, 6))                 # random number from 1 to 6, like a dice
print(random.choice(["rock", "paper", "scissors"]))

numbers = [1, 2, 3, 4, 5]
random.shuffle(numbers)
print(numbers)
```

### datetime (built in)

For working with dates and time.

```
from datetime import datetime

now = datetime.now()
print(now)
print(now.year)
print(now.strftime("%d-%m-%Y"))    # formatted date
```

### NumPy

Short for "Numerical Python". It is used for fast calculations on arrays of numbers, and is the base for most data science and machine learning libraries. A NumPy array is much faster than a normal python list for maths.

```
import numpy as np

a = np.array([1, 2, 3, 4])
b = np.array([10, 20, 30, 40])

print(a + b)          # [11 22 33 44]   works on every element at once
print(a * 2)          # [2 4 6 8]
print(a.mean())       # 2.5
print(np.zeros(3))    # [0. 0. 0.]
print(np.arange(0, 10, 2))   # [0 2 4 6 8]

matrix = np.array([[1, 2], [3, 4]])
print(matrix.shape)   # (2, 2)
```

Notice that `a + b` adds each pair of elements. With normal lists, `+` would just join the two lists together.

### Matplotlib

Used to make graphs and charts. We usually import its `pyplot` module as `plt`.

```
import matplotlib.pyplot as plt

x = [1, 2, 3, 4, 5]
y = [1, 4, 9, 16, 25]

plt.plot(x, y)
plt.title("Squares")
plt.xlabel("Number")
plt.ylabel("Square")
plt.show()
```

Matplotlib works great together with NumPy:

```
import numpy as np
import matplotlib.pyplot as plt

x = np.linspace(0, 10, 100)
y = np.sin(x)

plt.plot(x, y)
plt.title("Sine Wave")
plt.show()
```

Other common chart types:

```
plt.bar(["A", "B", "C"], [5, 8, 3])      # bar chart
plt.scatter([1, 2, 3], [4, 5, 6])        # scatter plot
plt.hist([1, 2, 2, 3, 3, 3, 4])          # histogram
```

### Pandas

Used for working with tables of data (like an Excel sheet or a CSV file).

```
import pandas as pd

data = {"Name": ["Aryan", "Riya", "Sam"], "Marks": [85, 92, 78]}
df = pd.DataFrame(data)

print(df)
print(df["Marks"].mean())

# reading a CSV file
# df = pd.read_csv("students.csv")
```

### Requests

Used to talk to websites and API's (see the API file for more on this).

```
import requests

response = requests.get("https://api.github.com")
print(response.status_code)    # 200 means success
```

## Lambda (not a library, but a useful keyword)

`lambda` isn't a library, it is a keyword for making small, one line, nameless functions. It is mentioned here since it is used very often along with built in functions that come from libraries. The format is `lambda inputs: output`.

A normal function:

```
def square(x):
    return x * x
```

The same thing using lambda:

```
square = lambda x: x * x
print(square(5))    # 25
```

With more than one input:

```
add = lambda a, b: a + b
print(add(3, 4))    # 7
```

Lambda is most useful when passed into functions like `map()`, `filter()` and `sorted()`:

```
numbers = [1, 2, 3, 4, 5, 6]

print(list(map(lambda x: x * 2, numbers)))          # [2, 4, 6, 8, 10, 12]
print(list(filter(lambda x: x % 2 == 0, numbers)))  # [2, 4, 6]

students = [("Aryan", 85), ("Riya", 92), ("Sam", 78)]
print(sorted(students, key=lambda s: s[1]))         # sorted by marks
```

## Useful Helpers

If you are not sure what a library contains, python can tell you:

```
import math

print(dir(math))      # lists everything inside math
help(math.sqrt)       # shows what sqrt does
```

## Making Your Own Module

Any python file you write can be imported as a library. If you have a file called `mytools.py`:

```
# mytools.py
def greet(name):
    return "Hello " + name
```

Then in another file in the same folder:

```
import mytools

print(mytools.greet("Aryan"))
```
