title: Hadoop CDH4 + cloudera + Vagrant
date: 2013-05-10 20:54:56
tags: [hadoop]
---
## 繼上次的教學

我們這次使用 cloudera 來安裝
版本: CDH4.2.1
OS: Ubuntu Precise (12.04) - Long-Term Support (LTS) 64-bit

<!-- more -->

## [Vagrant](http://www.vagrantup.com/)
教學參考：[用VM才是好的工程師-vagrant篇(入門版)](http://eva0919.github.io/2013/04/26/%E7%94%A8vm%E6%89%8D%E6%98%AF%E5%A5%BD%E7%9A%84%E5%B7%A5%E7%A8%8B%E5%B8%AB-vagrant%E7%AF%87%E5%85%A5%E9%96%80%E7%89%88/)
挑選 VM box [http://www.vagrantbox.es/](http://www.vagrantbox.es/)

```
# 下載安裝 vm ，等候五分鐘
$ vagrant box add precise64 http://files.vagrantup.com/precise64.box
# 查看下載了哪些 box
$ vagrant box list
# 初始化
$ vagrant init precise64
```

設定 `vi Vagrantfile`
```
Vagrant.configure("2") do |config|
  config.vm.box = "precise64"
  config.vm.network :forwarded_port, guest: 80, host: 8080
  config.vm.network :private_network, ip: "192.168.33.10"
  config.vm.network :public_network
end
```


```
# 啟動 vm
$ vagrant up
# 檢查啓動狀態
$ vagrant status
# => default                  running (virtualbox)
# 登入使用
$ vagrant ssh
# => Welcome to Ubuntu 12.04 LTS (GNU/Linux 3.2.0-23-generic x86_64)
```

一個乾淨到不行的 Ubuntu Precise (12.04) - Long-Term Support (LTS) 64-bit 產生！

```
$ cat /proc/version
# =>
Linux version 3.2.0-23-generic (buildd@crested) (gcc version 4.6.3 (Ubuntu/Linaro 4.6.3-1ubuntu4) ) #36-Ubuntu SMP Tue Apr 10 20:39:51 UTC 2012
```

## Install the Oracle Java Development Kit
```
# 先更新 apt-get
$ sudo apt-get update
$ sudo apt-get install openjdk-7-jre-headless
$ java -version
# => 
java version "1.7.0_21"
OpenJDK Runtime Environment (IcedTea 2.3.9) (7u21-2.3.9-0ubuntu0.12.04.1)
OpenJDK 64-Bit Server VM (build 23.7-b01, mixed mode)
```

設定環境變數 
```
# /usr/lib/jvm/java-1.7.0-openjdk-amd64/
$ export JAVA_HOME=/usr/lib/jvm/java-1.7.0-openjdk-amd64/
$ export PATH=$JAVA_HOME/bin:$PATH
$ echo $JAVA_HOME
# => /usr/lib/jvm/java-1.7.0-openjdk-amd64/
```

## Installing CDH4 on a Single Linux Node in Pseudo-distributed Mode
### 自動安裝  install CDH and Cloudera Manager Free Edition




