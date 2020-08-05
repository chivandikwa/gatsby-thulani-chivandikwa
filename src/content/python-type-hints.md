---
layout: post
title: Python Typehints - The complete guide
image: img/minimal.jpg
author: Thulani S. Chivandikwa
date: 2020-04-01T00:00:00.000Z
tags: [python, typehints, mypy, typing]
draft: false
---

This is my simple to follow type hints guide. Each type hint is introduced with an easy to follow example and explanation.

### Built in types

Any type, built in or user defined can be used as a type hint. Below are some of the common built in types


```python

integer: int = 10
float_value: float = 10.5
boolean: bool = True
string: str = 'hello there'
byte_array: bytearray = bytearray([1, 2])
bytes_value: bytes = bytes([1, 2])
complex_number: complex = complex(10, 5)
mapping: dict = {'x': 10}
# an immutable unordered collection of unique elements
frozen_set: frozenset = frozenset([1, 2])
# mutable sequence
list_collection: list = [1, 2]
memory_view: memoryview = memoryview(bytearray([1, 2]))
# the most base type
object_value: object = object()
# n unordered collection of unique elements
set_collection: set = {1, 2, 3}
# immutable sequence
tuple_collection: tuple = 2, 5
type_value: type = type(integer)
# a zip object whose .__next__() method returns a tuple where
# the i-th element comes from the i-th iterable argument
zip_object: zip = zip([1, 2], ['A', 'B'])
# the enumerate object yields pairs containing a count (from start, which
# defaults to zero) and a value yielded by the iterable argument
enumerate_object: enumerate = enumerate([1, 2])


```


### Any

This is the 'catch all' type hint you should avoid using unless in the process of porting code to support type hints. This simply means the variable/argument can be of any type.

> - Any is compatible with every type.
> - Any assumed to have all methods.
> - All values assumed to be instances of Any.

> object is superficially similar to Any at a glance but different to Any. Object is the root of Python's metaclass hierarchy but is still restrictive in that the only methods you are permitted to call are ones that are a part of the object definition, unlike any. Any is an escape hatch meant to allow you to mix together dynamic and statically typed code.

> This type is invalid in other positions, e.g., <code>List[NoReturn]</code>

```python

from typing import Any

# variable can be assigned to any type
unconstrained: Any
unconstrained = 10


# argument passed can be of any type
# and method can return any type
def sample(unconstrained: Any) -> Any:
    pass

```


### NoReturn

Special type indicating that a function never returns as in case of function that loops infinitely or always raises an exception.


```python

from typing import NoReturn


# this will never return
def raise_always() -> NoReturn:
    raise RuntimeError()


```



### Callable

Reference to a function or lambda that can be called.


```python

from typing import Callable


def to_int(value: str) -> int:
    return int(value)


# constrained callable accepting one string arg and return integer
convert_to_int: Callable[[str], int] = to_int
# unconstrained callable accepting any number of args of any type and returning any
double: Callable = lambda x: x * 2
# return type constrained callable accepting any number of args of any type and return nothing
print_out: Callable[..., None] = lambda x: print(x)

print_out(double(convert_to_int('250')))
# 500

print(callable(convert_to_int))
print(callable(double))
print(callable(print_out))
# True
# True
# True


```