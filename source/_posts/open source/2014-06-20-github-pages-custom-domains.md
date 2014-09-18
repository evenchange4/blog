title: GitHub Pages Custom Domains
date: 2014-06-20 01:49:59
tags: [github, page]
permalink: github-pages-custom-domains
layout: true
---

這邊為一個記錄將 Github Page 自定義到你所擁有的 Domain 下的一個流程。當然這些流程在 Goolge 搜尋 Github Page 就可以直接找到英文的說明文件，非常的詳細，此處就當做漢化的以及我自己碰到情境的過程記錄。

- CASE I:  `http://evenchange4.github.io` -> `http://michaelhsu.tw`
- CASE II: `http://about.michaelhsu.tw` -> `http://evenchange4.github.io/about`

<!-- more -->

## Basic 預備知識與情境說明
GitHub Pages 分為兩種：
1. User Pages: 例如 [http://evenchange4.github.io](http://evenchange4.github.io)，每個帳號都只有一個，必須命名為 `evenchange4.github.io`，且在 `master` branch。
2. Project Pages: 例如 [http://evenchange4.github.io/about](http://evenchange4.github.io/about)，也就是你的 GitHub 每一個 repository 都可以有一個 Page，必須在 `gh-pages` branch。

你可以注意到上面兩個例子可以分別導到我所擁有的 Domain name `michaelhsu.tw`，這是怎麼做到的呢？以下兩個情境依序帶你做設定。我這邊使用的 DNS provider 是 `Godaddy`。


## CASE I: A record to User Pages
也就是例子中的將 [http://evenchange4.github.io](http://evenchange4.github.io) 導到 [http://michaelhsu.tw](http://michaelhsu.tw)。

1. 在 [repository 中的 evenchange4.github.com](https://github.com/evenchange4/evenchange4.github.com) `master` branch 新增 `CNAME` 檔案，內容為一行 `michaelhsu.tw`
2. 將 Godaddy 新增 A record to 以下兩個 IP (兩個都要)
	- `192.30.252.153`
	- `192.30.252.154`

![▲ Figure: A record to User Pages](http://media-cache-ec0.pinimg.com/originals/c8/60/5e/c8605ef5757a8e5b33fdb1f41df4a806.jpg)

*需跳別注意 `207.97.227.245` or `204.232.175.78` 是舊的。*


## CASE II: ALIAS to Project Pages
大致上 CASE I 兩步驟設定完畢就可以直接由 `http://about.michaelhsu.tw` 導到 `http://evenchange4.github.io/about`，但是如果我想要自定義到  該怎麼做？

1. 在 [repository 中的 about](https://github.com/evenchange4/about) `gh-pages` branch 新增 `CNAME` 檔案，內容為一行 `about.michaelhsu.tw` 
2. 將 Godaddy 新增 CNAME record to `evenchange4.github.io`。

![▲ Figure: ALIAS to Project Pages](http://media-cache-ec0.pinimg.com/736x/bb/61/5d/bb615d925996f0401c5e2ec00bce6e64.jpg)

最後可以用 `dig` 來檢查：

```
$ dig about.michaelhsu.tw +nostats +nocomments +nocmd +multiline
=>
; <<>> DiG 9.8.3-P1 <<>> about.michaelhsu.tw +nostats +nocomments +nocmd +multiline
;; global options: +cmd
;about.michaelhsu.tw.	IN A
about.michaelhsu.tw.	3600 IN	CNAME evenchange4.github.io.
evenchange4.github.io.	3600 IN	CNAME github.map.fastly.net.
github.map.fastly.net.	19 IN A	199.27.79.133

```

## Reference
- [Tips for configuring an A record with your DNS provider](https://help.github.com/articles/tips-for-configuring-an-a-record-with-your-dns-provider)
- [Adding a CNAME file to your repository](https://help.github.com/articles/adding-a-cname-file-to-your-repository)
- [My custom domain isn't working](https://help.github.com/articles/my-custom-domain-isn-t-working)
- [User, Organization, and Project Pages](https://help.github.com/articles/user-organization-and-project-pages)
- [About custom domains for GitHub Pages sites](https://help.github.com/articles/about-custom-domains-for-github-pages-sites#subdomains)
