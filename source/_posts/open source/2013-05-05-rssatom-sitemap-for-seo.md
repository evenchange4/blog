title: RSS/Atom、Sitemap for SEO
date: 2013-05-05 17:51:30
tags: 
permalink: rssatom-sitemap-for-seo
---
> 不會動的，編成 sitemap.xml，會動的，用 rss 提交。 

> ![sitemap](http://3.bp.blogspot.com/-x-lZtfOu5rU/Tokfsiby51I/AAAAAAAAEPc/4ixGhkOKwDQ/s1600/sitemap.jpg)

## 一、首先瞭解 RSS 跟 Atom 差別在哪？
1. ATOM is an IETF standard while RSS is not
2. ATOM feeds explicitly indicates the content while the browser is left to figure out whether the RSS feed contains plain text or escaped HTML
3. ATOM code is modular and reusable while RSS code is not
4. RSS still holds dominance in the syndication format due to its head start and popularity

<!-- more -->

## 二、他們怎麼知道我網頁的 RSS/Atom link?
像是通常你要訂閱 blog ，可能在 RSS reader 輸入你的網址就會自動搜尋，他是怎麼做到的？

![Feedly](http://i.imgur.com/j3LIruF.png)

其實偷看一下 `<head>` ，其中的 type 是 `"application/rss+xml"` 或 `"application/atom+xml"`

- new blog demo: [http://evenchange4.github.io/atom](http://evenchange4.github.io/atom)

```
<head>
...
  <link rel="alternate" href="/atom.xml" title="Michael Hsu.tw" type="application/atom+xml">
...
</head>
```

- old blog demo: [http://evenchange4.tumblr.com/rss](http://evenchange4.tumblr.com/rss)

```
<head>
...
  <link rel="alternate" type="application/rss+xml" href="http://evenchange4.tumblr.com/rss">
...
</head>
```

## 三、那 Sitemap 呢？
> Sitemap 的提交主要的目的，是要避免搜索引擎的爬蟲沒有完整的收錄整個網頁的內容，所以提交 Sitemap 是能夠補足搜索引擎的不足，進而加速網頁的收錄速度，達到搜尋引擎友好的目的。

- demo: [http://michaelhsu.tw/sitemap](http://michaelhsu.tw/sitemap)

## 四、我碰到的問題，`<head>` 內的 link type RSS/Atom 錯亂？
當 RSS reader 自動搜尋 http://michaelhsu.tw/ 會偵測到 `/rss` 而不是 `/atom`，我覺得原因是我這個 domain name 之前是 DNS A(host) 到舊的 blog 那時候是用 rss，而現在新的 blog 是用 atom。
所以我覺得並不是因為 head 內寫錯了，而是有 CDN 的原因？~~待解決...~~
已解決，改成 feedburner 一勞永逸

```r
<link rel="alternate" href="http://feeds.feedburner.com/michael-hsu-tw" title="Michael Hsu.tw" type="application/atom+xml">
```

## 五、怎麼產生 RSS/Atom.XML
我的 blog 是 hexo node.js base 的，npm 有一個相當簡單的 package `hexo-generator-feed`，設定教學文可以參考 kpman | code 的[在hexo自訂rss](http://code.kpman.cc/2013/05/08/%E5%9C%A8hexo%E8%87%AA%E8%A8%82rss/)。

## Reference
- What is the difference between RSS and ATOM feeds? [stackoverflow](http://stackoverflow.com/questions/6619717/what-is-the-difference-between-rss-and-atom-feeds)
- Sitemap / RSS / Atom, SEO [stackoverflow](http://stackoverflow.com/questions/11006823/sitemap-rss-atom-seo)
- [瀏覽器自動偵測網站是否具有RSS訂閱](http://niucp03.blogspot.tw/2010/09/rss.html)
- [如何有效正確的提交Sitemap給搜索引擎](http://blog.seo-tw.org/2011/10/sitemap.html)


