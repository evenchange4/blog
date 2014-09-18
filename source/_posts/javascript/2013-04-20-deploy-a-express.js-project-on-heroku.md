title: Deploy a Express.js project on Heroku
date: 2013-04-21 01:10:57
tags: [node.js]
---
# [Demo](http://myfirstexpress.herokuapp.com/)
## Quick start
### Create a [Node.js](http://nodejs.org/) web framework '[express](http://expressjs.com/)'
```
$ express myfirstexpress && cd myfirstexpress
```
<!-- more -->
### Declare dependencies with NPM `/package.json` 
```
# /package.json
{
  "name": "application-name",
  "version": "0.0.1",
  "private": true,
  "scripts": {
    "start": "node app"
  },
  "dependencies": {
    "express": "3.0.0rc4",
    "jade": "*"
  },
  "engines": {
    "node": "0.8.x",
    "npm": "1.1.x"
  } 
}
```

### Install your dependencies (-l 處理相依性)
```
$ sudo npm install -l
````

### Declare process types with `Procfile`
```
# create '/Procfile'
web: node app.js

# test start
$ foreman start
```

### Git
```
$ git init
$ git add .
$ git commit -m "init"
```

### Create Heroku app
```
$ heroku create myfirstexpress
```
### Deploy on Heroku 
```
$ git push heroku master
```
### Run server
```
$ heroku ps:scale web=1
Scaling web processes... done, now running 1

# check server
$ heroku ps
=== web: `node app.js`
web.1: up for 10s
```
# Open
```
$ heroku open
http://myfirstexpress.herokuapp.com/
```
## Reference
[Getting Started with Node.js on Heroku](https://devcenter.heroku.com/articles/nodejs#listing-and-scaling-dynos)