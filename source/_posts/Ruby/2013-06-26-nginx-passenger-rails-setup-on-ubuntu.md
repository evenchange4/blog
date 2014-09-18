title: Nginx + Passenger + Rails4 Setup on Ubuntu12.04
date: 2013-06-26 17:48:41
tags: [ruby, nginx, passenger, rails, linux]
permalink: nginx-passenger-rails-setup-on-ubuntu
---

因為最近要架一個系統，想說從來沒有自己 host server ，就把流程都記錄一下吧。

![據說比 Apache 快 數十倍的利器](http://www.maesr.com/wp-content/uploads/2012/05/nginx_passenger_eyecatcher.png)

## 目錄
- 一、環境設定 Linux + Rails
- 二、環境設定 Nginx (via passenger RubyGems)
- 三、環境設定 mysql（for Mac Hombrew）
- 四、部署設定 mina
- 五、資安處理

<!-- more -->

## 一、環境設定 Linux + Rails
- SSH 登入: 

```
$ ssh evenchange4@hostip
```

- os: Ubuntu 12.04.2 LTS (GNU/Linux 3.2.0-23-generic x86_64)
- [rvm](https://rvm.io/) install: 

```
$ \curl -L https://get.rvm.io | bash -s stable --rails # Or, --ruby=1.9.3
```

- 檢查環境

```
$ rails -v
Rails 4.0.0

$ ruby -v
ruby 2.0.0p195 (2013-05-14 revision 40734) [x86_64-linux]

$ rvm list
rvm rubies
=* ruby-2.0.0-p195 [ x86_64 ]

```

- install [git](https://www.digitalocean.com/community/articles/how-to-install-git-on-ubuntu-12-04):

```
sudo apt-get install git
```

- rails server [ExecJS::RuntimeUnavailable error](http://stackoverflow.com/questions/15515180/execjsruntimeunavailable-error-when-i-starts-rails-server):

```
$ gem install therubyracer
$ sudo apt-get install nodejs
```

## 二、環境設定 Nginx (via passenger RubyGems)
>> Nginx (pronounced engine-x) is a free, open-source, high-performance HTTP server and reverse proxy.

-![反向代理概念](http://ihower.tw/rails3/images/deployment-proxy-diagram.jpg)

- 其中 Web 伺服器可以是 Apache、Nginx，但是它除了提供靜態檔案之外，其餘的任務就只是做 reverse proxy 將 request 分發到應用程式伺服器。

- 安裝 nginx proxy server [配置：2.2 (via passenger RubyGems)](http://www.modrails.com/documentation/Users%20guide%20Nginx.html#_supported_operating_systems) 

```
$ gem install passenger

$ chmod o+x /home/evenchange4/.rvm/gems/ruby-2.0.0-p195/gems/passenger-4.0.5/
$ chmod o+x /home/evenchange4/.rvm/gems/ruby-2.0.0-p195/gems
$ chmod o+x /home/evenchange4/.rvm/gems/ruby-2.0.0-p195
$ chmod o+x /home/evenchange4/.rvm/gems
$ chmod o+x /home/evenchange4/.rvm
$ chmod o+x /home/evenchange4

$ sudo apt-get install libcurl4-openssl-dev
$ rvmsudo passenger-install-nginx-module

```

- 設定 [Nginx](http://ruby-china.org/wiki/mac-nginx-passenger-rails): `$ sudo vi /opt/nginx/conf/nginx.conf` ，Add a server block inside `http` block

```
...
   server {
      listen 80;
      server_name 127.0.0.1;
      root /home/evenchange4/test123/public;
      passenger_enabled on;
      rails_env development; # 切換
      #rails_env production;
   }
...
```

- 上方 block 可以透過 [rails_env](http://www.modrails.com/documentation/Users%20guide%20Nginx.html) 來轉換環境。

- Create an Init Script to Manage nginx: [Init script for Ubuntu 12.04](https://library.linode.com/web-servers/nginx/installation/ubuntu-12.04-precise-pangolin#sph_create-an-init-script-to-manage-nginx)

```
$ wget -O init-deb.sh http://library.linode.com/assets/1139-init-deb.sh
$ sudo mv init-deb.sh /etc/init.d/nginx
$ chmod +x /etc/init.d/nginx
$ sudo /usr/sbin/update-rc.d -f nginx defaults

```

- start/stop/restart nginx 

```
$ sudo /etc/init.d/nginx start/stop/restart
```

- nginx status

```
$ passenger-memory-stats
```

## 三、環境設定 mysql

- Install（Mac Hombrew）:

```
$ brew install mysql
```

- Install（Ubuntu）:

```
$ sudo apt-get install mysql-server mysql-common mysql-client libmysqlclient-dev
```

- run mysql server

```
# mac
$ mysql.server start/stop

# ubuntu
$ sudo service mysql start
```

- login 

```
$mysql -u root -p
```

- create database

```
mysql> CREATE DATABASE ass_development;
mysql> show databases;
```

- 檢查 mysql 是否支援中文 utf8
	1. 更改 /etc/mysql/my.cnf [詳細教學](http://blog.sina.com.cn/s/blog_4bc179a80100hmjc.html)
	2. [砍掉 db](http://stackoverflow.com/questions/16350310/mysql-mysql2error-incorrect-string-value)

- 匯入 (必須需要有database name)

```
$ mysql -uroot -pmcc2012 NTUMB_development < code/2013-04-24_10_45.NTUMB_development
```

- new [RailsApps](https://github.com/RailsApps/rails3-bootstrap-devise-cancan) project，選擇 `I want to build my own application` 來作設定。

- production

```
$ rake db:drop RAILS_ENV=production
$ rake db:create RAILS_ENV=production
$ rake db:migrate RAILS_ENV=production
$ rake db:seed RAILS_ENV=production
$ sudo /etc/init.d/nginx restart
```


## 四、部署設定 [mina](http://nadarei.co/mina/)
- 因為 [capistrano](https://github.com/capistrano/capistrano) 非常缺乏文檔資料，加上不太支援 Rails4 ，所以挑選 mina 來幫助部署。
- 透過 mina 的輔助，可以將 source code（須先 commit 到 github上） 透過 ssh 連線自動部署到 hosting server 上。並且可以直接寫 `rake` [script](https://github.com/evenchange4/ass/blob/master/config/deploy.rb) 的指令來隨心所欲的執行指令。
- ![mina](http://nadarei.co/mina/images/logo.png?1344377458)

```
$ mina init
```

- 如果 [ssh 無反應](https://github.com/nadarei/mina/issues/99)，`vi config/depoy.rb` 加入:

```
# Password prompt in mina doesn't work
set :term_mode, nil
```

```
$ mina setup
```

- 自行到 server 編輯 `database.yml`

```
$ git commit -a -m "deploy"
$ git push
$ mina deploy
```

## 五、資安處理
- 安裝 [denyhosts](https://www.digitalocean.com/community/articles/how-to-install-denyhosts-on-ubuntu-12-04) 防止 ssh 帳號被連續 try，並且加入白名單
- [paperclip generate access_token and interpolate it](https://github.com/thoughtbot/paperclip/wiki/Interpolations)

## Reference
1. [rvmsudo passenger-install-nginx-module](http://askubuntu.com/questions/169551/why-cant-i-install-phusion-passenger-for-nginx)
2. [YamlDb](https://github.com/ludicast/yaml_db)
