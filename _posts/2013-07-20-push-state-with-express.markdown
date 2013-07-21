---
layout: post
title: Push State with Express
category: posts
type: text
---

I had some trouble getting push-state set up on heroku, so I figured I would share.  
For those unfamiliar, push state allows you to manipulate the the url in the browser without refreshing the page. 
This is great for client side apps which had to rely on `location.hash` in the past.

To get it working I ended up using express and it was actually pretty simple.

```coffeescript
express = require 'express'
path = require 'path'
 
app = express()
 
app.configure ->
  app.use express.static(__dirname + '/public')

app.all '*', (request, response) ->
  response.sendfile './public/index.html'

 
port = process.env.PORT || port
console.log "startServer on #{port}"
app.listen port

```

The api for my app was on another domain, but you should be able to add routes before the 
`app.all '*'` without issue.  Hope this helps someone in the future.