title: IE8 JS function 宣告方式 （SCRIPT 445: 物件不支援此動作。）
date: 2013-08-13 11:38:51
tags: [ie, ie8, js]
---

最近在 whoscall 實習幫忙寫一些網頁功能，雖然平常也不太會認真寫，但是真的在應用的時候還是需要去顧及很多方面可能會出現的錯誤。真的沒想到只有寫簡單的 js、jquery 也能爆掉，在 chrome、firefox、Safari 測試都沒有問題，再測試 IE 的時候 ie10、ie9 看起來也還不錯，但是唯獨 ie8 爆掉了，後來發現應該是 `javascript function 宣告的方式` 造成的。


> SCRIPT 445: 物件不支援此動作。


![ie8](http://media-cache-ak0.pinimg.com/originals/14/9d/9c/149d9ca713845fe28d88f8a7b90759ff.jpg)

<!-- more -->

寫法一：這是原本的寫法，ie8 會爆掉

```
change_message = function(message){
	...
};
```

寫法二：這是後來的寫法

```
function change_message(message){
	...
};
```

原本我剛接觸 javascript 的時候都是寫`寫法二`的方式，不過很多時候看別人的 code 都會出現`寫法一`的宣告方式，感覺很潮於是也就有樣學樣，那時候也沒想太多為什麼大家都不加 `var`，沒想到會出現爆掉的情況。

所以能夠小潮有安全的寫法應該要這樣：

```
var change_message = function(message){
	...
};
```

總之，期待這個小功能可以順利上線！

## 補充
1. ie7 的 "JSON" object 問題： [IE 說「'JSON' 未被定義」!?](http://kelp.phate.org/2011/01/ie-json.html)
