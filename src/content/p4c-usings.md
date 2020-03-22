---
layout: post
title: Python for C# developers series (Using Directives)
image: img/animal1.jpg
author: Thulani S. Chivandikwa
date: 2020-03-22T00:00:00.000Z
tags: [.NET, C#, Python, Context Managers, Usings, Using Directive]
draft: false
---

In my path to mastering python I decided what better way to do so that to teach. To make it even more interesting I am taking this from the perspective of how does that feature in C# work in python, given i would do this in C# this way, how can I do it in Python? This is the first post of a series, looking at using directives in C#, context managers in Python.

### Using Directives in Python (Context Managers)

Context Managers allow you to allocate and release resources precisely when you want to. They in particular help in guaranteeing release of resources even if an error occurs.

Imagine this scenario:

```python
file = open('source.tct', 'w')
file.write('Some text')
file.close()
```

If an error occurs while writing to the file the code to close the file resource would not be called. One way to addres this is as follows:

```python
file = open('source.tct', 'w')
try:
    file.write('Some text')
finally:
    file.close()
```

The context managers provide syntactic sugar to do just this as follows:

```python
with open('source.txt', 'w') as opened_file:
    opened_file.write('Some text')
```

Behind the scenses something interesting is happening, let's have a look at how to implement context managers that will make this clear. This can be done in two ways.

### Implementing Context Managers as classes

At the very least a context manager has an <code>\_\_enter\_\_</code> and <code>\_\_exit**</code> method defined which are executed and beginning and end of scope (context). If an exception occurs, Python passes the type, value and traceback of the exception to the <code>\_\_exit**</code> method. If <code>\_\_exit\*\*</code> returns True then the exception was gracefully handled.

Let's consider a scoped context manager that executes a one lambda on start of the scope and another on end of the scope (context).

```python
from typing import Callable


class ScopedActions(object):
    startup: Callable[[], None]
    cleanup: Callable[[], None]
    name: str

    def __init__(self, name: str, startup: Callable[[], None], cleanup: Callable[[], None]):
        self.name = name
        self.startup = startup
        self.cleanup = cleanup

    def __enter__(self):
        self.startup()
        return self

    def __exit__(self, type, value, traceback):
        self.cleanup()


with ScopedActions(name='test', startup=lambda: print('started'), cleanup=lambda: print('finished')) as scoped:
    print('Executing:', scoped.name)
```

### Implementing Context Managers as generators

Context managers can also be implementing in a more succint way using generators as functions two parts, the context start and context end separater by the yield keyword. Exceptions in this scenario can be handled by a traditional try statement which is also more intuitive. This makes use of the <code>@contextmanager</code> decorator.

```python
from contextlib import contextmanager
from typing import Callable


@contextmanager
def scoped_actions(name: str, startup: Callable[[], None], cleanup: Callable[[], None]):
    startup()
    yield name
    cleanup()


with scoped_actions(name='test', startup=lambda: print('started'), cleanup=lambda: print('finished')) as name:
    print('Executing:', name)
```

Checkout the complete code sample used in this gist [on github](https://gist.github.com/chivandikwa/9c45bd9be009383ec911465fc21035c1).
