---
layout: post
title: Importing curl requests in Postman
image: img/best/water-tower.jpg
author: Thulani S. Chivandikwa
date: 2020-04-04T00:00:00.000Z
tags: [Postman, curl, request]
draft: false
---

I often have requests shared in various documentation like in Confluence and it can be annoying for the next person when they have to manually make a request in Postman (current tool of choice) to run it particularly when it has many headers and an intricate body etc.

Fortunately I found a way to extract curl code from Postman which I then save in the documentation and when this needs to be run it is very trivial to copy import this back into Postman.

> There may be better ways to share requests across teams with collections. This is more powerful for the paid version of Postman. Sharing requestes with a set of people you may not know up front and for the free version is easer with curl. Furthermore there are many other ways one can run the curl other than Postman.

### Sample request

Here is a sample request:

Endpoint: https://localhost:44369/api/dummy/
Request Type: POST
Headers: [target: cron, full_run: true, user: anonymous, Content-Type: application/json]
Body: { "job": "FTP Cleanup", "delay": 0 }

| | |
| :------------: | :------------: |
|   Endpoint  |   https://localhost:44369/api/dummy/   |
|   Request   |   POST   |
|   Headers   |   [target: cron, full_run: true, user: anonymous, Content-Type: application/json]  |
|   Body   |   { "job": "FTP Cleanup", "delay": 0 }   |

To export locate the code option under the Save button as shown below

![curl-export](https://raw.githubusercontent.com/chivandikwa/gatsby-thulani-chivandikwa/master/src/content/img/postman-curl-export.jpg)

This is the resulting curl

```python

curl --location --request POST 'https://localhost:44369/api/dummy/' \
--header 'target: cron' \
--header 'ful_-run: true' \
--header 'user: anonymous' \
--header 'Content-Type: application/json' \
--data-raw '{
    "job": "FTP Cleanup",
    "delay": 0
}'



```

Importing is super easy. In the top left corner select Import and ensure that Raw Text is selected. Paste the curl as shown below. Click continue and confirm the import details


![curl-export](https://raw.githubusercontent.com/chivandikwa/gatsby-thulani-chivandikwa/master/src/content/img/postman-curl-import.jpg)

This should now make for easy and happy sharing of requests 😁