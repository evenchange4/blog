title: Use Express and Mongolab to create Todo-list on Heroku
date: 2013-04-21 02:10:34
tags: [node.js]
---
# [Demo](http://mongolab-todo-list.herokuapp.com/)
## Quick start
### Clone an [express-todo-example](https://github.com/dreamerslab/express-todo-example) code

```
$ git clone https://github.com/dreamerslab/express-todo-example.git mongolab-todo-list
```

<!-- more -->

### Link MongoDB Hosting `/db.js` 
1. create a [MongoLab Platform account](https://mongolab.com/about/products/)
2. get 0.5 GB Free
3. To connect using a driver via the standard URI 

```
mongoose.connect( 'mongodb://localhost/express-todo' );
```

### Deploy on Heroku
Check another Michael's [gist](https://gist.github.com/3773179).

## Reference
1. [Express Todo Example](https://github.com/dreamerslab/express-todo-example)
2. [用 Express 和 MongoDB 寫一個 todo list](http://dreamerslab.com/blog/tw/write-a-todo-list-with-express-and-mongodb/)
3. [MongoLab Platform](https://mongolab.com/home) 
4. [[NodeJS] Use NoSQL Database mongoDB on Heroku - MongoLab Platform](http://cire.pixnet.net/blog/post/37418936)
5. [Deploy a Express.js project on Heroku](https://gist.github.com/3773179)
6. [Coding 趴的意外收穫](https://docs.google.com/presentation/d/1OkDskY801NVXFyxOK7TQ7zscFdhALMplTtWCbZn1mQk/edit#slide=id.p)