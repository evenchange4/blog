title: 為什麼你該使用 Linux Screen
date: 2013-12-21 12:35:27
tags: [linux, ssh]
permalink: use-linux-screen
---

對於 Linux 系統幾乎都是內建的工具，可以讓你真正的遠端工作，最適合有使用遠端主機透過 ssh 或是其他連線方式進行工作的人，讓你下次回到工作模式時，立即進入狀況！


> 讓你下次回到工作模式時，立即進入狀況！

<!-- more -->


## 常用指令
-  Out of screen

	```
	$ screen -r
	$ screen -ls
	```

-  In screen `ctrl+a` 之後鍵入：
	- `c`: create a window
	- `d`: 離開 (detach)
	- `A`: rename the window
	- `k`: kill
	- `?`: 熱鍵查詢
	- `數字`: 換頁

## 配置設定 Screen 畫面 `vim ~/.screenrc`

```
hardstatus on
hardstatus alwayslastline
hardstatus string "%{.bW}%-w%{.rY}%n %t%{-}%+w %=%{..G} %H(%l) %{..Y} %Y/%m/%d %c:%s "
# 設定視窗回捲時可看到的行數
defscrollback 2048
```

- ![linux screen ](http://media-cache-ak0.pinimg.com/originals/36/73/42/3673423624133dff9416119ed50d2853.jpg)


## Reference
- [沒事做，弄點小設定。把常用的screen 再弄得看起來厲害一點](http://napmas.blogspot.tw/2011/09/screenrc.html)