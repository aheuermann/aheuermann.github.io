---
layout: post
title: Push State with Node.js and Express
category: posts
type: text
---

I spent some time getting a single page app with push-state set up on heroku, so I figured I would share how I did it. For those unfamiliar, push state allows you to manipulate the url in the browser without refreshing the page. This is great for client-side apps which had to rely on `location.hash` in the past.

This doesn't seem like much of a server-side issue, and it isn't until the user tries to do something like bookmark the page or send it to a friend.  Obviously, the server doesn't know by default that `http://example.com/posts` is a client-side route.  It will try to match the route and you will likely end up with a 404.  So the key is getting any route to load your index page while still allowing static assets such as javascript, css, images, fonts, ect.. to be loaded.

To get it working I ended up using express and it was actually pretty simple.  Here is my server script.

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
Also, you should be able to add routes before the `app.all '*'` without issue. Hope this helps someone in the future.