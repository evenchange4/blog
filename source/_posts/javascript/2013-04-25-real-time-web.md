title: Real Time Web
date: 2013-04-25 05:46:09
tags: [node.js]
permalink: real-time-web
---
## Node.js

### 是什麼：
1. build scalable network applications using JavaScript on the server-side.
2. 據 JavaScript Runtime 寫成，因為大部份都是 C code ，所以很快。

### 可以做什麼：
1. Websocket Server （像聊天室）
2. Fast File Upload Client
3. Any Real-Time Data Apps

<!-- more -->

### 不是什麼：
1. A Web Framework
2. Multi-threaded（可視為 single threaded server ）

## Blocking Code vs Non-Blocking Code（callback）

* Blocking

一般為了做:

1. Calls out to web services
2. Reads/Writes on the Database 
3. Calls to extensions

```
var contents = fs.readFileSync('/etc/hosts');
console.log(contents);
console.log('Doing something else');
```

* Non-Blocking（callback）

```
fs.readFile('/etc/hosts', function(err, contents) {
  console.log(contents);
});
console.log('Doing something else');
```

### 語法

```
fs.readFile('/etc/hosts', function(err, contents) {
  console.log(contents);
});
```

* ￼￼￼Same as

```
var callback = function(err, contents) {
  console.log(contents);
}
fs.readFile('/etc/hosts', callback);
```

### Event
將 Event 附加在 DOM 上。

 
## [Websocket 101](http://lucumr.pocoo.org/2012/9/24/websockets-101/)
### What
1. 因為 bidirectional communication 的需求。
2. 如果兩邊終端（endpoints）非瀏覽器則不一定需要使用 Websocket 這個 protocol。
3. work with existing HTTP infrastructure，所以受到很多限制。
4. 不同於單純的 raw TCP connection，因為命名的關係常常聯想成傳統的 socket。事實上他在於訊息的傳輸是根據 UDP，但卻有 TCP 的 reliable， 

websocket adds on top of that:

1. upgrades from HTTP 1.1
2. Through the HTTP handshake

### data structure
為了要存放 non-blocking message，使用 [Redis](https://github.com/antirez/redis)。
> Redis is an in-memory database that persists on disk. The data model is key-value, but many different kind of values are supported: Strings, Lists, Sets, Sorted Sets, Hashes 