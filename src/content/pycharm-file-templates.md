---
layout: post
title: Pycharm new file templates
image: img/best/zeeland-bridge.jpg
author: Thulani S. Chivandikwa
date: 2020-04-01T00:00:00.000Z
tags: [python, pycharm, template]
draft: false
---

I have a few custom new file templates that I have setup in Pycharm. Adding/Editing is easy (File -> Settings [ctrl + alt + s] -> Editor -> File and Code Templates) 

![settings](https://raw.githubusercontent.com/chivandikwa/gatsby-thulani-chivandikwa/master/src/content/img/pycharm_file_templates.png)

### Unittest unit test


```python

import unittest

class #[[$Title$]]#Tests(unittest.TestCase):
    def test_x_given_when_then(self):
        self.assertEqual(True, False)

if __name__ == '__main__':
    unittest.main()



```


### Sanic API


```python

import logging

from sanic import Sanic, response
from sanic.request import Request
from sanic.response import HTTPResponse
from sanic_openapi import doc, swagger_blueprint
from sanic_openapi.doc import summary

logger = logging.getLogger(__name__)
app = Sanic(__name__)
app.logger = logger

app.blueprint(swagger_blueprint)
app.config['API_SCHEMES'] = ['http', 'https']
app.config['API_TITLE'] = APP_NAME
app.config['API_DESCRIPTION'] = ''


@summary('')
@doc.produces(doc.List(doc.String))
@app.get('/test')
async def test(_: Request) -> HTTPResponse:
    return response.json(['a', 'b'])


if __name__ == '__main__':
    app.run(host=HOST, port=PORT, threaded=True)


def api() -> Sanic:
    return app



```