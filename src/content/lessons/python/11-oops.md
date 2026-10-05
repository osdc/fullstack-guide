---
course: python
slug: oops
title: Python Object Oriented Programming
description: "Learn classes, objects, methods, constructors, inheritance, encapsulation, and object-oriented programming in Python."
---

Object-Oriented Programming, commonly called **OOP**, is a programming style that organizes code around objects.

An object combines:

- **Data**, stored in attributes
- **Behavior**, defined by methods

For example, an OSDC workshop can have data such as its name and number of seats. It can also have behavior such as registering a member or displaying its details.

# Why Use OOP?

Object-oriented programming helps you:

- Organize related data and functions together
- Reuse code through inheritance and composition
- Represent real-world entities in code
- Protect and validate object data
- Build larger applications that are easier to maintain

Python supports multiple programming styles, including procedural, functional, and object-oriented programming. OOP is especially useful for larger applications, APIs, databases, and frameworks such as FastAPI.

# Classes and Objects

A **class** is a blueprint for creating objects. An **object** is an individual instance created from a class.

```python
class Workshop:
    pass


python_workshop = Workshop()
fastapi_workshop = Workshop()
```

Here:

- `Workshop` is a class
- `python_workshop` is an object of the `Workshop` class
- `fastapi_workshop` is another object of the same class

Objects created from the same class can have different data.

# The `__init__()` Method

The `__init__()` method is a special method that runs automatically when an object is created. It is commonly used to initialize object attributes.

```python
class Workshop:
    def __init__(self, name, duration):
        self.name = name
        self.duration = duration


python_workshop = Workshop("Python Workshop", 2)

print(python_workshop.name)
print(python_workshop.duration)
```

The `__init__()` method receives the values passed during object creation and stores them in the object.

# The `self` Parameter

`self` refers to the current object. It allows each object to store and access its own attributes and methods.

```python
class Workshop:
    def __init__(self, name):
        self.name = name


python_workshop = Workshop("Python Workshop")
fastapi_workshop = Workshop("FastAPI Workshop")

print(python_workshop.name)
print(fastapi_workshop.name)
```

`self.name` belongs to the current object. The `name` parameter is the value received while creating that object.

When calling a method, Python passes the object as the `self` argument automatically:

```python
class Workshop:
    def show_name(self):
        print(self.name)


workshop = Workshop()
# workshop.show_name()  # AttributeError because name was not initialized
```

A method should always include `self` as its first parameter unless it is a static method.

# Instance Attributes

Instance attributes belong to a particular object. Different objects can have different values for the same attribute.

```python
class Member:
    def __init__(self, name, skill):
        self.name = name
        self.skill = skill


member_one = Member("OSDC Member 1", "Python")
member_two = Member("OSDC Member 2", "JavaScript")

print(member_one.name)
print(member_one.skill)
print(member_two.name)
print(member_two.skill)
```

Changing one object's attribute does not change another object's attribute:

```python
member_one.skill = "FastAPI"

print(member_one.skill)
print(member_two.skill)
```

# Instance Methods

A method is a function defined inside a class. Instance methods can read and modify the current object's attributes.

```python
class Workshop:
    def __init__(self, name, seats):
        self.name = name
        self.seats = seats

    def show_details(self):
        print(f"Workshop: {self.name}")
        print(f"Available seats: {self.seats}")


workshop = Workshop("Python and FastAPI", 40)
workshop.show_details()
```

Methods can update object state:

```python
class Workshop:
    def __init__(self, name, seats):
        self.name = name
        self.seats = seats

    def register_member(self):
        if self.seats > 0:
            self.seats -= 1
            print("Registration successful.")
        else:
            print("The workshop is full.")


workshop = Workshop("Python Workshop", 2)
workshop.register_member()
workshop.register_member()
workshop.register_member()

print("Remaining seats:", workshop.seats)
```

# Class Attributes

A class attribute is shared by all objects created from the class.

```python
class Workshop:
    platform = "OSDC at JIIT, Noida"

    def __init__(self, name):
        self.name = name


python_workshop = Workshop("Python")
fastapi_workshop = Workshop("FastAPI")

print(python_workshop.platform)
print(fastapi_workshop.platform)
```

Class attributes are useful for values shared by every object.

```python
class Workshop:
    total_workshops = 0

    def __init__(self, name):
        self.name = name
        Workshop.total_workshops += 1


Workshop("Python")
Workshop("FastAPI")

print(Workshop.total_workshops)
```

Do not use a class attribute for data that should be independent for each object.

# Instance Attributes vs Class Attributes

```python
class Club:
    institution = "JIIT, Noida"  # class attribute

    def __init__(self, name):
        self.name = name  # instance attribute


osdc = Club("OSDC")

print(osdc.name)
print(osdc.institution)
```

Use `self.attribute` for object-specific data and `ClassName.attribute` for shared class data.

# Updating and Deleting Attributes

Attributes can be updated after an object is created.

```python
class Workshop:
    def __init__(self, name, seats):
        self.name = name
        self.seats = seats


workshop = Workshop("Python Workshop", 40)
workshop.seats = 35

print(workshop.seats)
```

The `del` statement can remove an attribute:

```python
del workshop.seats
```

Accessing the deleted attribute later raises an `AttributeError`. In larger programs, methods or properties are usually preferred for controlled updates.

# Encapsulation

Encapsulation means keeping data and the operations that use that data together. It also means controlling how an object's internal state is accessed or changed.

Python uses naming conventions rather than strict access modifiers:

- `name`: public attribute
- `_name`: protected-by-convention attribute
- `__name`: name-mangled attribute intended to reduce accidental access

## Public Attributes

Public attributes can be accessed directly.

```python
class Workshop:
    def __init__(self, name):
        self.name = name


workshop = Workshop("Python Workshop")
print(workshop.name)
```

## Protected-by-Convention Attributes

A single leading underscore tells other developers that an attribute is intended for internal use.

```python
class Workshop:
    def __init__(self, name):
        self._name = name


workshop = Workshop("Python Workshop")
print(workshop._name)  # Possible, but usually treated as internal
```

Python does not make a single-underscore attribute truly private.

## Name-Mangled Attributes

A double leading underscore triggers name mangling.

```python
class Workshop:
    def __init__(self, name):
        self.__name = name

    def get_name(self):
        return self.__name


workshop = Workshop("Python Workshop")
print(workshop.get_name())
```

Name mangling helps prevent accidental access and name conflicts in subclasses. It is not a complete security mechanism.

# Properties

A property allows a method to be used like an attribute. Properties are useful for validation and controlled access.

```python
class Workshop:
    def __init__(self, name, seats):
        self.name = name
        self._seats = seats

    @property
    def seats(self):
        return self._seats

    @seats.setter
    def seats(self, value):
        if value < 0:
            raise ValueError("Seats cannot be negative.")
        self._seats = value


workshop = Workshop("Python Workshop", 40)
print(workshop.seats)

workshop.seats = 35
print(workshop.seats)

# workshop.seats = -1  # ValueError
```

The `@property` method controls reading the value. The `@seats.setter` method controls assigning a new value.

A read-only property can be created without a setter:

```python
class Workshop:
    def __init__(self, name, duration):
        self.name = name
        self.duration = duration

    @property
    def summary(self):
        return f"{self.name} ({self.duration} days)"


workshop = Workshop("FastAPI Workshop", 2)
print(workshop.summary)
```

# Inheritance

Inheritance allows one class to reuse and extend another class.

- The parent class is also called the base or superclass.
- The child class is also called the derived or subclass.

```python
class Workshop:
    def show_category(self):
        print("OSDC workshop")


class PythonWorkshop(Workshop):
    pass


workshop = PythonWorkshop()
workshop.show_category()
```

`PythonWorkshop` inherits the `show_category()` method from `Workshop`.

## Adding Child-Specific Behavior

```python
class Workshop:
    def show_category(self):
        print("OSDC workshop")


class PythonWorkshop(Workshop):
    def show_language(self):
        print("Python")


workshop = PythonWorkshop()
workshop.show_category()
workshop.show_language()
```

## Calling the Parent Constructor with `super()`

Use `super()` to call a method from the parent class.

```python
class Workshop:
    def __init__(self, name):
        self.name = name


class OnlineWorkshop(Workshop):
    def __init__(self, name, meeting_link):
        super().__init__(name)
        self.meeting_link = meeting_link


workshop = OnlineWorkshop(
    "FastAPI Workshop",
    "https://example.com/osdc-fastapi"
)

print(workshop.name)
print(workshop.meeting_link)
```

Without `super().__init__(name)`, the `name` attribute would not be initialized by the parent constructor.

## Overriding Methods

A child class can provide its own version of a method inherited from the parent. This is called method overriding.

```python
class Workshop:
    def show_format(self):
        print("Workshop format is not specified.")


class OnlineWorkshop(Workshop):
    def show_format(self):
        print("This workshop is online.")


class InPersonWorkshop(Workshop):
    def show_format(self):
        print("This workshop is held at JIIT, Noida.")


OnlineWorkshop().show_format()
InPersonWorkshop().show_format()
```

# Multiple Inheritance

A class can inherit from more than one parent class.

```python
class TechnicalEvent:
    def show_technical_details(self):
        print("This is a technical event.")


class CommunityEvent:
    def show_community_details(self):
        print("This event is organized by a community.")


class OSDCWorkshop(TechnicalEvent, CommunityEvent):
    pass


event = OSDCWorkshop()
event.show_technical_details()
event.show_community_details()
```

Multiple inheritance can be useful, but it should be used carefully. Simple composition is often easier to understand when classes represent separate components.

# Polymorphism

Polymorphism means that the same operation can work with objects of different classes.

```python
class OnlineWorkshop:
    def conduct(self):
        print("Conducting the workshop online.")


class InPersonWorkshop:
    def conduct(self):
        print("Conducting the workshop at JIIT, Noida.")


def start_workshop(workshop):
    workshop.conduct()


start_workshop(OnlineWorkshop())
start_workshop(InPersonWorkshop())
```

The `start_workshop()` function does not need to know the exact class. It only expects the object to provide a compatible `conduct()` method.

## Duck Typing

Python commonly follows the idea: if an object behaves like the required type, it can be used.

```python
class PythonWorkshop:
    def start(self):
        print("Starting the Python workshop.")


class FastAPIWorkshop:
    def start(self):
        print("Starting the FastAPI workshop.")


def start_event(event):
    event.start()


start_event(PythonWorkshop())
start_event(FastAPIWorkshop())
```

Neither class needs to inherit from a common class. Both work because they provide a `start()` method.

# Abstraction

Abstraction means exposing the important interface while hiding implementation details.

Python provides abstract base classes through the `abc` module.

```python
from abc import ABC, abstractmethod


class Workshop(ABC):
    @abstractmethod
    def conduct(self):
        pass


class PythonWorkshop(Workshop):
    def conduct(self):
        print("Conducting the Python workshop.")


workshop = PythonWorkshop()
workshop.conduct()
```

A class with an abstract method cannot be instantiated until its child class implements that method.

```python
# workshop = Workshop()  # TypeError
```

Abstract classes are useful when several classes must follow the same interface.

# Composition

Composition means building a class by containing objects of other classes. It represents a **has-a** relationship.

Inheritance represents an **is-a** relationship:

- A `PythonWorkshop` is a `Workshop`.

Composition represents a **has-a** relationship:

- An `OSDCEvent` has a `Venue`.

```python
class Venue:
    def __init__(self, name):
        self.name = name


class OSDCEvent:
    def __init__(self, title, venue):
        self.title = title
        self.venue = venue

    def show_details(self):
        print(f"{self.title} at {self.venue.name}")


venue = Venue("JIIT, Noida")
event = OSDCEvent("Full Stack Workshop", venue)
event.show_details()
```

Composition often keeps classes smaller and more flexible than a deep inheritance hierarchy.

# Special Methods

Special methods, also called magic or dunder methods, start and end with double underscores. Python calls them automatically in specific situations.

## `__str__()`

The `__str__()` method defines the human-readable representation of an object.

```python
class Workshop:
    def __init__(self, name, duration):
        self.name = name
        self.duration = duration

    def __str__(self):
        return f"{self.name} ({self.duration} days)"


workshop = Workshop("Python Workshop", 2)
print(workshop)
```

Without `__str__()`, printing the object usually shows a less useful representation containing its class and memory address.

## `__repr__()`

The `__repr__()` method is intended to provide an unambiguous representation useful for debugging.

```python
class Workshop:
    def __init__(self, name, duration):
        self.name = name
        self.duration = duration

    def __repr__(self):
        return f"Workshop(name={self.name!r}, duration={self.duration!r})"


workshop = Workshop("Python Workshop", 2)
print(repr(workshop))
```

## `__len__()`

The `__len__()` method defines what `len()` should return for an object.

```python
class WorkshopSchedule:
    def __init__(self, workshops):
        self.workshops = workshops

    def __len__(self):
        return len(self.workshops)


schedule = WorkshopSchedule(["Python", "FastAPI"])
print(len(schedule))
```

# Class Methods

A class method receives the class as its first argument, conventionally named `cls`. Use the `@classmethod` decorator.

```python
class Workshop:
    platform = "OSDC at JIIT, Noida"

    def __init__(self, name):
        self.name = name

    @classmethod
    def show_platform(cls):
        print(cls.platform)


Workshop.show_platform()
```

Class methods can also act as alternative constructors:

```python
class Workshop:
    def __init__(self, name, duration):
        self.name = name
        self.duration = duration

    @classmethod
    def from_text(cls, text):
        name, duration = text.split(",")
        return cls(name.strip(), int(duration))


workshop = Workshop.from_text("Python Workshop, 2")
print(workshop.name)
print(workshop.duration)
```

# Static Methods

A static method belongs to a class but does not receive `self` or `cls`. Use the `@staticmethod` decorator when the operation is related to the class but does not need object or class data.

```python
class Workshop:
    @staticmethod
    def is_valid_seat_count(seat_count):
        return seat_count > 0


print(Workshop.is_valid_seat_count(40))
print(Workshop.is_valid_seat_count(0))
```

A static method can be called through the class or an object, but it does not depend on either one.

# Dataclasses

For classes that mainly store data, the `dataclasses` module can generate common methods such as `__init__()` and `__repr__()`.

```python
from dataclasses import dataclass


@dataclass
class Workshop:
    name: str
    duration: int
    is_online: bool = False


workshop = Workshop("Python Workshop", 2)
print(workshop)
```

Dataclasses are useful for structured data such as API request and response models. FastAPI also commonly uses classes with type annotations to describe data.

# Object Relationships

Common relationships between classes include:

| Relationship | Meaning | Example |
|---|---|---|
| Inheritance | An object is a type of another object | `PythonWorkshop` is a `Workshop` |
| Composition | An object contains another object | An event has a `Venue` |
| Aggregation | An object uses another object that can exist independently | A schedule contains workshops |
| Association | Objects interact with each other | A member registers for a workshop |

Choosing the right relationship makes an application easier to understand and change.

# OOP Example: Workshop Registration

The following example combines classes, attributes, methods, validation, and composition.

```python
class Member:
    def __init__(self, name):
        self.name = name
        self.registered_workshops = []

    def register(self, workshop):
        if workshop.register_member(self):
            self.registered_workshops.append(workshop.name)
            print(f"{self.name} registered for {workshop.name}.")
        else:
            print(f"No seats available for {workshop.name}.")


class Workshop:
    def __init__(self, name, seats):
        self.name = name
        self.seats = seats
        self.members = []

    def register_member(self, member):
        if self.seats <= 0:
            return False

        self.seats -= 1
        self.members.append(member.name)
        return True


member = Member("OSDC Member")
python_workshop = Workshop("Python Workshop", 1)

member.register(python_workshop)
member.register(python_workshop)

print("Registered workshops:", member.registered_workshops)
print("Remaining seats:", python_workshop.seats)
```

# Common Mistakes

## Forgetting `self`

```python
class Workshop:
    def __init__(name):
        # Incorrect: self is missing
        pass
```

Correct version:

```python
class Workshop:
    def __init__(self, name):
        self.name = name
```

## Confusing Class and Instance Attributes

```python
class Workshop:
    platform = "OSDC"

    def __init__(self, name):
        self.name = name
```

`platform` is shared by the class, while `name` belongs to each individual object.

## Forgetting Parent Initialization

```python
class Workshop:
    def __init__(self, name):
        self.name = name


class OnlineWorkshop(Workshop):
    def __init__(self, name, link):
        super().__init__(name)
        self.link = link
```

Call `super().__init__()` when the child class needs the parent class to initialize its attributes.

## Modifying Internal Data Without Validation

```python
class Workshop:
    def __init__(self, seats):
        self._seats = seats

    @property
    def seats(self):
        return self._seats

    @seats.setter
    def seats(self, value):
        if value < 0:
            raise ValueError("Seats cannot be negative.")
        self._seats = value
```

Properties or methods can protect an object from invalid state.

## Creating One Giant Class

A class should have a focused responsibility. Separate unrelated responsibilities into separate classes and use composition when appropriate.

# Quick Reference

```python
class ClassName:
    class_attribute = "shared value"

    def __init__(self, value):
        self.instance_attribute = value

    def instance_method(self):
        return self.instance_attribute


object_name = ClassName("value")
print(object_name.instance_method())
```

| Concept | Meaning |
|---|---|
| Class | Blueprint for objects |
| Object | Instance of a class |
| Attribute | Data stored on an object or class |
| Method | Function defined inside a class |
| `self` | Current object |
| `__init__()` | Initializes a new object |
| Inheritance | Reuses and extends another class |
| Encapsulation | Controls access to object data |
| Polymorphism | Same operation works with different object types |
| Abstraction | Exposes an interface while hiding details |
| Composition | Builds an object from other objects |
| `@property` | Provides controlled attribute access |
| `@classmethod` | Method that receives the class |
| `@staticmethod` | Method that receives neither the object nor class |
