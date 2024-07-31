# Coding Standards and Best Practices

Since we are a method development group, we need to produce *production level* code. If you have not had much experience with any of the following, it is worth the time investment to get up to speed - not only so your project will be more stable, usable and maintainable, but **to give you skills that future employers want to see, guaranteed.**

## Paradigm: OOP

Object-oriented programming (OOP) powerful and the standard in production development environments. Reminder of the main features and benefits below. Ensure you are integrating the all of the following principles in your code!

- **Modularity and encapsulation:** OOP allows you to break down complex programs into smaller, more manageable pieces, or modules. Each module can have its own data and functionality, making it easier to work on and test in isolation. Encapsulation ensures that the internal workings of a module are hidden from other parts of the program, reducing the likelihood of bugs and making it easier to modify and update the code.
- **Code reuse:** OOP encourages the creation of reusable code, which can save time and effort in the long run. Objects can be created once and used multiple times, reducing the amount of code that needs to be written and maintained.
- **Inheritance and polymorphism:** OOP allows you to create new objects based on existing objects, through inheritance. This can save time and effort in creating new code, and also helps ensure consistency across the program. Polymorphism allows objects to take on multiple forms, depending on the context, which can make code more flexible and extensible. This allows you to easily iterate on your tool development to test different features.
- **Abstraction:** OOP allows you to create abstract data types that capture the essence of a concept, without getting bogged down in implementation details. This can make code more understandable and easier to reason about.

## Typing and Commenting

It is important for your code to be readable and maintainable. Ensure you are integrating all of the following:

- **Commenting**
    - Use comments to explain why code is written in a certain way, not what the code is doing.
    - Avoid commenting every line of code, instead focus on complex or non-obvious sections of the code.
    - Use descriptive names for variables and functions to reduce the need for comments.
    - Use inline comments sparingly and only when necessary.
    - Use a consistent commenting style throughout the codebase.
- **Typing**
    - Use type hints for function arguments, return values, and class attributes.
    - Use Union types to indicate that a function can accept multiple types of arguments.
    - Use Optional types for arguments that are not required.
- **Docstrings**
    - Use docstrings to describe the purpose and behavior of functions and classes.
    - Follow the [Google style guide](https://sphinxcontrib-napoleon.readthedocs.io/en/latest/example_google.html) for writing docstrings.
    - Start the docstring with a one-line summary of the function or class.
    - Include information on function arguments, return values, and exceptions that can be raised.
    - Include examples of how to use the function or class.

💡

Circular imports can be a problem when typing. Use the following methods to avoid it:

1. Using the following type check

```python
from typing import TYPE_CHECKING

if TYPE_CHECKING:
    from models import Book
```

1. Use `from __future__ import annotations` and **put your typing in single quotes**

## Testing

WIP

## Docs

WIP