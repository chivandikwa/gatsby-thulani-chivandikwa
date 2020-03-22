---
layout: post
title: Python for C# developers series (Async Using Directives)
image: img/animal2.jpg
author: Thulani S. Chivandikwa
date: 2020-03-22T00:00:00.001Z
tags: [.NET, C#, Python, Context Managers, Usings, Using Directive, Async, IAsyncDisposable]
draft: false
---

Continuing on the last post about Context Managers (using directives) I will extend this to how it works with async code. The idea of async disposables is fairly new to C#, introduced to C# 8.

### Implementing async Context Managers as classes

At the very least a context manager has an <code>\_\_aenter\_\_</code> and <code>\_\_aexit**</code> method defined which are executed and beginning and end of scope (context). If an exception occurs, Python passes the type, value and traceback of the exception to the <code>\_\_aexit**</code> method. If <code>\_\_aexit\*\*</code> returns True then the exception was gracefully handled.

Let's consider a scoped context manager that executes a one lambda on start of the scope and another on end of the scope (context).

```python
import asyncio
from typing import Callable


class ScopedActions(object):
    startup: Callable[[], None]
    cleanup: Callable[[], None]
    name: str

    def __init__(self, name: str, startup: Callable[[], None], cleanup: Callable[[], None]):
        self.name = name
        self.startup = startup
        self.cleanup = cleanup

    async def __aenter__(self):
        await asyncio.sleep(1)
        self.startup()
        return self

    async def __aexit__(self, type, value, traceback):
        await asyncio.sleep(1)
        self.cleanup()


async def test():
    async with ScopedActions(name='test', startup=lambda: print('started'),
                             cleanup=lambda: print('finished')) as scoped:
        print('Executing:', scoped.name)


loop = asyncio.new_event_loop()
result = loop.run_until_complete(test())
```

### Implementing async Context Managers as generators

Context managers can also be implementing in a more succint way using generators as functions two parts, the context start and context end separater by the yield keyword. Exceptions in this scenario can be handled by a traditional try statement which is also more intuitive. This makes use of the <code>@asynccontextmanager</code> decorator.

```python
import asyncio
from contextlib import asynccontextmanager
from typing import Callable


@asynccontextmanager
async def scoped_actions(name: str, startup: Callable[[], None], cleanup: Callable[[], None]):
    startup()
    await asyncio.sleep(1)
    yield name
    cleanup()


async def test():
    async with scoped_actions(name='test', startup=lambda: print('started'), cleanup=lambda: print('finished')) as name:
        print('Executing:', name)


loop = asyncio.new_event_loop()
result = loop.run_until_complete(test())
```

Checkout the complete code sample used in this gist [on github](https://gist.github.com/chivandikwa/9c45bd9be009383ec911465fc21035c1).
