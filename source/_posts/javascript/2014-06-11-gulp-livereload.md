title: Gulp.js-LiveReload 自動刷新頁面
date: 2014-06-11 23:52:54
tags: [gulp, node.js]
permalink: gulp-livereload
layout: true
---

最近又重回學習前端的懷抱，在看[蛋頭兄的 Angular.js](https://egghead.io/) 的影片邊操作的同時，不斷的 Refresh 網頁真的不是辦法，以前我的想法是就不停的按 `CMD+R` 就算了，懶得做那麼多自動刷新得設定，但這樣下去真的不是辦法，原來還是會累的，所以就依照目前最潮的方式來學習設置「自動刷新頁面」的功能。於是乎挑選了 `Gulp.js + LiveReload` with `http-server` ，或是考慮使用 `gulp-connect`。

- Source Code: [https://github.com/evenchange4/gulp-livereload](https://github.com/evenchange4/gulp-livereload)

<!-- more -->

# Knowledge 前置小作業
對於使用的工具名稱至少要有一認識，所以查了一下資料。
- [Gulp.js](http://gulpjs.com/): 看影片念做 `告Ｐ`，是一個自動化構建的工具。
- [Gulp.js](http://gulpjs.com/) vs [Grunt](http://gruntjs.com/): 最主要差在 Gulp 使用 node.js 的 Streams 來加速。
- [LiveReload 2](http://livereload.com/): Client 端的整合自動刷新利器。
- 靜態網頁可以嗎？：不行，所以這裡使用 node 簡易的 http server "[http-server](https://github.com/nodeapps/http-server)"，後面會一併教大家如何使用。
- [gulp-connent](https://github.com/avevlad/gulp-connect): 把 webserver 跟 livereload 弄在一起的套件。

# 選擇使用 gulp-livereload with http-server
## 第一步：Install 安裝 

Install Node Package， 這邊用 `npm` 來下載管理使用到的套件：

```
$ sudo npm install gulp -g
$ sudo npm install http-server -g
```

Install Chrome Extension，安裝 [Chrome Extension: LiveReload](https://chrome.google.com/webstore/detail/livereload/jnihajbhpnppcggbcgedagnkighmdlei)，作為 client 端監控的插件。

![▲ Figure: Goolge Chrome Extension: LiveReload](http://media-cache-ak0.pinimg.com/originals/68/1d/3b/681d3be7a8e194b1b99f29c5eb98ec2a.jpg)

## 第二步：Config 前置作業與設定 

Managing module dependencies，讓後人執行可以一目了然：

```
$ npm init
$ sudo npm install gulp gulp-livereload --save-dev
```

Gulp Config，配置設定 Gulp，新增且編輯 `gulpfile.js`：

```
var gulp = require('gulp'),
	livereload = require('gulp-livereload');

gulp.task('watch', function () {  // 'watch' 是 task 的名稱，可以任意定義名稱  
	var server = livereload();

	
    gulp.watch('*.*', function (file) {	// 監控的檔案，在這裏 '*.*' 我監控所有檔案
        server.changed(file.path);
    });
});
```

## 第三步：Quick Start 執行

運行 Run http-server，因為前面提到的，直接用瀏覽器開啟的檔案靜態網頁不支援，因此一定要有一個 http server，所以這邊用非常簡單 node 的 `http-server` 來運作：

```
$ http-server
Starting up http-server, serving ./ on port: 8080
Hit CTRL-C to stop the server
...
```

運行 Run gulp-livereload 執行 Gulp 監聽的 Task: 

```
$ gulp watch
[00:22:57] Using gulpfile ~/code/node/livereload/gulpfile.js
[00:22:57] Starting 'watch'...
[00:22:57] Finished 'watch' after 7.41 ms
[00:23:23] index.html was reloaded.
[00:23:23] Live reload server listening on: 35729
```

連接 Chrome-livereload，在 Chrome 瀏覽器連接起來，這邊第一次連可能會沒反應，多點幾次就好了，使得`圈圈變為實心`的：

![▲ Figure: Goolge Chrome Extension：必須要為實心圈圈](http://media-cache-cd0.pinimg.com/originals/c6/84/7f/c6847f0f85412e6229563f5c556baf7d.jpg)

# 選擇使用 gulp-connect
因為 `gulp-connect` 這個 node 的套件已經將 webserver 整合在一起，所以不需要特別執行 http-server。程序同前面，我快速帶過。

## 第一步：Install 安裝
```
$ sudo npm install gulp-connect --save-dev
```
## 第二步：Config 前置作業與設定

將  `gulpfile.js` 編輯：

```
var gulp = require('gulp'),
  connect = require('gulp-connect');

gulp.task('connect', function() {
  connect.server({
    root: '',
    livereload: true
  });
});

gulp.task('html', function () {
  gulp.src('*.html')
    .pipe(connect.reload());
});

gulp.task('watch', function () {
  gulp.watch(['*.*'], ['html']);
});

gulp.task('default', ['connect', 'watch']);
```

## 第三步：Quick Start 執行

再來執行 `gulp` 即可：

```
$ gulp                                                          
[03:08:42] Using gulpfile ~/code/node/livereload/gulpfile.js
[03:08:42] Starting 'connect'...
[03:08:42] Server started http://localhost:8080
[03:08:42] LiveReload started on port 35729
[03:08:42] Finished 'connect' after 18 ms
[03:08:42] Starting 'watch'...
[03:08:42] Finished 'watch' after 7.29 ms
[03:08:42] Starting 'default'...
[03:08:42] Finished 'default' after 7.06 μs
```

## Reference
- [Gulp.js—比Grunt更易用的前端构建工具](http://www.36ria.com/6373)
- [使用gulp及livereload实时更新页面](https://www.youtube.com/watch?v=OKVE6wE9CW4)
- [Gulp.js-livereload 不用F5了，实时自动刷新页面来开发](http://angularjs.cn/A0xJ)
- [The streaming build system Gulp 比較](http://blog.wu-boy.com/2013/12/streaming-build-system-gulp/)
- [gulp-connect config](https://github.com/avevlad/gulp-connect#livereload)
