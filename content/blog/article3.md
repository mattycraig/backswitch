---
title: Article Number 3
category: articles
description: Lorem
exceprt: Lorem
tags: Learn, More, Syrup
image: blah.jpg
---

## Lorem ipsum

This is another article blah blah :heart: :-)

```js{1,3-5}[server.js]
const http = require('http')
const bodyParser = require('body-parser')

http.createServer((req, res) => {
  bodyParser.parse(req, (error, body) => {
    res.end(body)
  })
}).listen(3000)
```
