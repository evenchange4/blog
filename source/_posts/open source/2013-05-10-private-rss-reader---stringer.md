title: Host self RSS reader - Stringer
date: 2013-05-10 05:01:05
tags: [ruby]
---
剛剛在 [ruby weekly](http://rubyweekly.com/) 看到這個玩具，看標題就感覺十分的有趣馬上來玩玩看！

> # Stringer - 
> A [work-in-progress] self-hosted, anti-social RSS reader. 

是一個私人的 RSS reader 然後可以 host 在 heroku 上！蠻好奇他是怎麼設計的。

<!-- more -->

## 下載安裝使用
- stringer 在 [https://github.com/swanson/stringer](https://github.com/swanson/stringer) 有完整的步驟。

- demo: [http://mreader.herokuapp.com/](http://mreader.herokuapp.com/)

呃，都說是私人的了，就沒辦法 demo 給你看囉 XD，不過可以看看截圖或是自己實際玩玩看：

![stringer demo](https://fbcdn-sphotos-b-a.akamaihd.net/hphotos-ak-ash4/262472_606814032665116_1927512106_n.jpg)

## 學習的地方
- RSS(xml) parser：[Feedzirra](https://github.com/pauldix/feedzirra)。
後來發現原來我有玩過，勉為其難看一下以前的 [blog](http://evenchange4.tumblr.com/post/32658941295/code-google-news) 吧。~~懶得轉了~~
- UI：Twitter Bootstrap and Flat UI.
- [heroku addons:add scheduler](https://addons.heroku.com/scheduler)：因為 RSS 需要不停地去更新，但是等到使用者要看才再去 refresh 就慢了，所以 heroku 上有一個可以定期的執行 cmd 指令的套件可以使用，每隔一段時間就去執行一次。
> ## Scheduler: clock Run Jobs with Ease
Scheduler is an add-on for running jobs on your app at scheduled time intervals, much like cron in a traditional server environment.

![heroku addons:add scheduler](http://i.imgur.com/LMIMnil.png)

然後到 heroku 上的 [scheduler dashboard](https://heroku-scheduler.herokuapp.com/dashboard) 作設定：

![Heroku Scheduler](http://i.imgur.com/gPQ4j8d.png)

- Rake：[Ruby make](http://rake.rubyforge.org/)：如此可以直接使用 cmd 指令來執行。
	- Rakefile：發佈和打包的 rake tasks
	```
	desc "Fetch all feeds."
	task :fetch_feeds do
	   FetchFeeds.new(Feed.all).fetch_all
	end
	```
	- 
	![rack](http://farm3.staticflickr.com/2793/4373500414_017aabf744_o.png)

- use subDomain CNANE：如此一來真的可以變成自己的 RSS reader 阿！（教學一樣在作者的 github 步驟上） 

- 幽默：他在 github 上面有句話，~~看來有人臉很腫啊~~。
> When `BIG_FREE_READER` shuts down, your instance of Stringer will still be kicking. 

## Reference 
- github [https://github.com/swanson/stringer](https://github.com/swanson/stringer)
- [Ruby][教學] 如何打包一個 Gem [Blog](http://blog.xdite.net/posts/2012/01/04/how-to-pack-a-gem/)
- [通过分析Rack和Rails的源代码, 讲解Rails的启动流程。](http://railscasts-china.com/episodes/the-rails-initialization-process-by-kenshin54)
- [Rack 與 Rack middleware](http://wp.xdite.net/?p=1557)
