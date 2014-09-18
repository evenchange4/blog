title: Rails on PostgreSQL
date: 2013-07-03 16:28:30
tags: [ruby, PostgreSQL]
permalink: rails-on-postgresql
---

![又是隻大象](http://jangmt.com/wiki/images/b/b5/PostgreSQL-9.gif)
最近看到老師都在玩 PostgreSQl ，看看我之前都是用 MySQL 當做資料庫的，感覺應該也來跟進下，雖然不前太知道他有什麼優缺點，但是學長聽說 MySQL 之後會閉源了，沒有人維護的確是很大的問題，總之就先來玩玩看吧。

## Install postgresql

- Mac: 請 100% 跟者 ”[How to Install PostgreSQL on a Mac With Homebrew and Lunchy](http://www.moncefbelyamani.com/how-to-install-postgresql-on-a-mac-with-homebrew-and-lunchy/)“ 動一動。

<!-- more -->


```
$ brew update
$ brew doctor
$ brew install postgresql
$ initdb /usr/local/var/postgres -E utf8
$ gem install lunchy
$ mkdir -p ~/Library/LaunchAgents
$ cp /usr/local/Cellar/postgresql/9.2.1/homebrew.mxcl.postgresql.plist ~/Library/LaunchAgents/
```

- Ubuntu

```
$ sudo apt-get install postgresql
```

- check & start

```
$ postgres -V
$ lunchy start postgres

# for ubuntu
$ sudo /etc/init.d/postgresql start

$ brew info postgres
```


## Setup postgresql

```
$ lunchy start postgres
$ lunchy stop postgres

# for ubuntu
$ sudo /etc/init.d/postgresql start
```

- add user

```
$ sudo su - postgres # for ubuntu error

$ createuser --createdb --pwprompt username
$ dropuser root
```

## Rails on PostgreSQL

- Gemfile

```
gem 'pg'
```

```
$ sudo aptitude install libpq-dev # error for ubuntu

$ bundel install
```

- config/database.yml

```
development:
  adapter: postgresql
  encoding: utf8
  database: ass_development
  pool: 5
  username: root
  password: 1234
  host: localhost

...
```

- Rake 

```
$ rake db:create:all
$ rake db:migration
$ rake db:seed
```

- for production

```
$ rake db:drop RAILS_ENV=production
$ rake db:create RAILS_ENV=production
$ rake db:migrate RAILS_ENV=production
$ rake db:seed RAILS_ENV=production
```

## Reference
- [Creating Rails users in Postgres on Ubuntu](http://mikewilliamson.wordpress.com/2010/12/21/creating-rails-users-in-postgres-on-ubuntu/)
- [How to Setup PostgreSQL for Rails and Heroku](http://robdodson.me/blog/2012/04/27/how-to-setup-postgresql-for-rails-and-heroku/)
- [[宅] 在Ubuntu安裝PostgreSQL](http://cat-son.blogspot.tw/2012/11/ubuntupostgresql.html#sthash.Dpjq9Kt8.MsdA7rZr.dpbs)
- [PostgreSQL error: Fatal: role “username” does not exist](http://stackoverflow.com/questions/11919391/postgresql-error-fatal-role-username-does-not-exist)