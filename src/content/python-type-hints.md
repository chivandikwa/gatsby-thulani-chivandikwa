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

### Any

This is the 'catch all' type hint you should avoid using unless in the process of porting code to support type hints. This simply means the variable/argument can be of any type.

> object is superficially similar to Any at a glance but different to Any. Object is the root of Python's metaclass hierarchy but is still restrictive in that the only methods you are permitted to call are ones that are a part of the object definition, unlike any. Any is an escape hatch meant to allow you to mix together dynamic and statically typed code.

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