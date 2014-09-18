title: R and Hadoop 整合初體驗
date: 2013-05-01 23:08:17
tags: [R, hadoop]
permalink: r-and-hadoop-初體驗
---
> ![R & Hadoop](http://i.imgur.com/IVgmGSJ.jpg)
> 當 R 尬上 Hadoop，到底會激盪出什麼樣的火花呢？下面實際跑過書上的範例來為您介紹，三種不一樣的整合方式任君挑選。


## 目錄
1. Hadoop & R
2. R and Streaming
3. Rhipe
4. RHadoop

<!-- more -->

## 安裝 R
想知道它是 Fedora, Debian, 還是Ubuntu 等等的 Linux distribution version：
```
$ cat /proc/version
# =>
Linux version 2.6.32-358.0.1.el6.x86_64 (mockbuild@c6b10.bsys.dev.centos.org) (gcc version 4.4.7 20120313 (Red Hat 4.4.7-3) (GCC) ) #1 SMP Wed Feb 27 06:06:45 UTC 2013
```

所以選擇安裝 Red Hat el6 => [官網](http://cran.rstudio.com/)竟然沒有，只好自己谷歌
安裝 [R-2.15.2-1.el6.x86_64.rpm](http://pkgs.org/centos-6-rhel-6/epel-x86_64/R-2.15.2-1.el6.x86_64.rpm/download/)。

## 安裝 Hadoop
參考 [Hadoop 第一次安裝就上手 CDH3](http://michaelhsu.tw/2013/04/21/hadoop-%E7%AC%AC%E4%B8%80%E6%AC%A1%E5%AE%89%E8%A3%9D%E5%B0%B1%E4%B8%8A%E6%89%8B-cdh3/)。

## 範例資料來源
下載 hadoop book example: [https://github.com/alexholmes/hadoop-book](https://github.com/alexholmes/hadoop-book)：

```
$ git clone git@github.com:alexholmes/hadoop-book.git
```

>## 整合一：R and Streaming
>*可以使用任何程式語言來實作 map/reduce，只要規定好標準的input/output。
好處：在本機端就可以很簡單的做測試，不需要真正去涉及 MapReduce 就能 test，並且透過 cmd 來執行 hadoop job。
這邊透過兩個例子（map-only、Full MapReduce）來演示。*

## 一、map-only job， problem: 計算 stocks 每日的平均
* 不 care join or group data in reducer。 
* 其中 input csv file 的 element 為 `Symbol,Date,Open,High,Low,Close,Volume,Adj Close`
* 日平均算法：開盤(open)跟收盤(close)的平均。


step1: 首先先在本機端（Mac）上跑看看：
```
# cat ｜ 表示 把前一次運行的結果往後面丟。
$ cat test-data/stocks.txt | src/main/R/ch8/stock_day_avg.R
# => 
AAPL	2009-01-02	88.315
AAPL	2008-01-02	197.055
AAPL	2007-01-03	85.045
...
```

step2: 接著丟上 hadoop 來看看：

```
# 首先，先新增一個環境變數，設定hadoop 的路徑
$ export HADOOP_HOME=/usr/java/hadoop-1.0.4/

# 移除 output file 內的資料
$ ${HADOOP_HOME}/bin/hadoop fs -rmr output

# 複製到 hadoop file system
$ ${HADOOP_HOME}/bin/hadoop fs -put test-data/stocks.txt \stocks.txt
# check 
$ ${HADOOP_HOME}/bin/hadoop fs -ls

# run hadoop mamp/reduce， -file: copy 到 distributed cache
$ ${HADOOP_HOME}/bin/hadoop \
  jar ${HADOOP_HOME}/contrib/streaming/*.jar \
  -D mapreduce.job.reduces=0 \
  -inputformat org.apache.hadoop.mapred.TextInputFormat \
  -input stocks.txt \
  -output output \
  -mapper `路徑要設`/src/main/R/ch8/stock_day_avg.R \
  -file `路徑要設`/src/main/R/ch8/stock_day_avg.R
# 我的路徑範例
$ /usr/java/hadoop-1.0.4/bin/hadoop   jar /usr/java/hadoop-1.0.4/contrib/streaming/hadoop-streaming-1.0.4.jar   -D mapreduce.job.reduces=0   -inputformat org.apache.hadoop.mapred.TextInputFormat   -input stocks.txt   -output output   -mapper src/main/R/ch8/stock_day_avg.R   -file src/main/R/ch8/stock_day_avg.R

# check ouput 從網頁看，或是 fs
$ ${HADOOP_HOME}/bin/hadoop fs -cat output/part*
# =>
AAPL	2008-01-02	197.055	
AAPL	2009-01-02	88.315	
AAPL	2007-01-03	85.045	
AAPL	2006-01-03	73.565	
AAPL	2005-01-03	64.035	
...
```

*結果與本機端相同。*

step3: 來看看 R 的 code on [github](https://github.com/alexholmes/hadoop-book/blob/master/src/main/R/ch8/stock_day_avg.R) ：
```r
# R process name
#! /usr/bin/env Rscript 

# 關掉 warnings
options(warn=-1)

# 切換 output 到 path
sink("/dev/null")

# standard input.
input <- file("stdin", "r")

# read n line，false 結尾為 empty，不需要是EOF
while(length(currentLine <- readLines(input, n=1, warn=FALSE)) > 0) {
   # split 後，unlist 把 list 轉換成 vector
   fields <- unlist(strsplit(currentLine, ","))

   lowHigh <- c(as.double(fields[3]), as.double(fields[6]))
   stock_mean <- mean(lowHigh)

   # 切換回來 output 到 terminal
   sink()

   # concatenate 和 ouput
   cat(fields[1], fields[2], stock_mean, "\n", sep="\t")
   
   # 切換回去
   sink("/dev/null")
}
close(input)
```


## 二、Full MapReduce job， problem: 計算 stocks [累積移動平均（CMA）](http://zh.wikipedia.org/wiki/%E7%A7%BB%E5%8B%95%E5%B9%B3%E5%9D%87)
* CMA 算法為：同樣的公司不同日期的平均。

step1: 同樣地先在本機端（Mac）上跑看看：
```
# cat ｜ 表示 把前一次運行的結果往後面丟。
# map | runtime sort（用 linux cmd 指令模仿的） | reduce
$ cat test-data/stocks.txt | src/main/R/ch8/stock_day_avg.R | sort --key 1,1 | src/main/R/ch8/stock_cma.R
# => 
AAPL  68.997
CSCO  30.8985
GOOG  419.943
MSFT  44.6725
YHOO  70.971
```

step2: 接著丟上 hadoop 來看看：

```
$ export HADOOP_HOME=/usr/java/hadoop-1.0.4/
$ ${HADOOP_HOME}/bin/hadoop fs -rmr output
$ ${HADOOP_HOME}/bin/hadoop fs -put test-data/stocks.txt \stocks.txt
$ ${HADOOP_HOME}/bin/hadoop \
  jar ${HADOOP_HOME}/contrib/streaming/*.jar \
  -inputformat org.apache.hadoop.mapred.TextInputFormat \
  -input stocks.txt \
  -output output \
  -mapper src/main/R/ch8/stock_day_avg.R \
  -reducer src/main/R/ch8/stock_cma.R \
  -file src/main/R/ch8/stock_day_avg.R \
  -file src/main/R/ch8/stock_cma.R
$ ${HADOOP_HOME}/bin/hadoop fs -cat output/part*
# =>
AAPL  68.997  
CSCO  49.94775  
GOOG  123.9468  
MSFT  101.297 
YHOO  94.55789  
```

![最後結果截圖](http://i.imgur.com/zJWwuXF.png) 
** 但其實，答案跟預期不同，似乎算錯了！**

step3: mapper 的 code 同 problem 1 的 mapper，所以我們來看看 R `reducer` 的 code on [github](https://github.com/alexholmes/hadoop-book/blob/master/src/main/R/ch8/stock_cma.R)：
```r
#! /usr/bin/env Rscript
options(warn=-1)
sink("/dev/null")

# R 定義 function，input means：vector
outputMean <- function(stock, means) {
  stock_mean <- mean(means)
  sink()
  cat(stock, stock_mean, "\n", sep="\t")
  sink("/dev/null")
}

input <- file("stdin", "r")

prevKey <- ""
means <- numeric(0)

while(length(currentLine <- readLines(input, n=1, warn=FALSE)) > 0) {

  fields <- unlist(strsplit(currentLine, "\t"))

  key <- fields[1]
  mean <- as.double(fields[3])

  if( identical(prevKey, "") || identical(prevKey, key)) {
    prevKey <- key
    means <- c(means, mean)
  } else {
    outputMean(prevKey, means)
    prevKey <- key
    
    # 這邊 sample code 寫錯，發現新的 key 應該要用新的 
    means <- numeric(0) vector

    means <- c(means, mean)
  }
}

if(!identical(prevKey, "")) {
  outputMean(prevKey, means)
}

close(input)
```

** 注意 line 33，更改後才會得到正確的答案。 **

![正確的答案](http://i.imgur.com/IUo5fqg.png)


>## 整合二：Rhipe
>Client-side R and Hadoop working together。
>「R and Hadoop Integrated Processing Envirnment」的縮寫，在 R 裡面運行 Hadoop job。


## 一、安裝 [Rhipe](http://saptarshiguha.github.io/RHIPE/installation.html)

先安裝 [Protocol Buffers](https://code.google.com/p/protobuf/)，下載`` protobuf-2.4.1.tar.gz ``，一定要這個版本，記得看 readme 安裝方法。

> Protocol Buffers are a way of encoding structured data in an efficient yet extensible format. Google uses Protocol Buffers for almost all of its internal RPC protocols and file formats.
> think XML, but smaller, faster, and simpler.

```
$ cd protobuf-2.4.1
$ ./configure
$ make
$ make check
$ make install
```

挑一個最多人下載的版本 [Rhipe_0.69.tar.gz](https://github.com/saptarshiguha/RHIPE/downloads)。
```
$ export PKG_CONFIG_PATH=/usr/local/lib/pkgconfig
$ export HADOOP=/usr/java/hadoop-1.0.4
$ export HADOOP_LIB=$HADOOP/lib
$ export HADOOP_CONF_DIR=$HADOOP/conf
$ /sbin/ldconfig
$ R CMD INSTALL /root/Downloads/Rhipe_0.69.tar.gz

# check install
$ R
> library(Rhipe)
```

## 二、同 problem 2 計算 stocks 累積移動平均（CMA）

```
$ export HADOOP_HOME=/usr/java/hadoop-1.0.4/
$ ${HADOOP_HOME}/bin/hadoop fs -rmr output
$ ${HADOOP_HOME}/bin/hadoop fs -put test-data/stocks.txt /tmp/stocks.txt
$ export HADOOP_BIN=/usr/java/hadoop-1.0.4/bin
$ hadoop-book-master/src/main/R/ch8/stock_cma_rhipe.R
```

![結果截圖](http://i.imgur.com/mJ66DfS.png)

~~接著看一下 整合 rhipe 的 R code on [github](https://github.com/alexholmes/hadoop-book/blob/master/src/main/R/ch8/stock_cma_rhipe.R)~~

## 三、問題討論
1. 亂碼？
2. ~~結果數據與第一個整合不一樣~~：已解決，問題出在整合一的 sample code 算錯。

>## 整合三：RHadoop
> 相較簡單的整合 Client-side R and Hadoop，同Rhipe可在 R 裡面運行 Hadoop job。

RHadoop其中包含了三個元件

1. rmr (整合 R、Hadoop) ，範例只專注在 rmr 上。
2. rdfs（R 的 HDFS 界面）
3. rhbase（R 的 Hbase 界面）

## 一、安裝
前置套件安裝
```r
$ R
> install.packages("RJSONIO")
> install.packages("itertools") 
> install.packages("digest")
> q()

$ R CMD javareconf
$ R
> install.packages("rJava")
> install.packages("Rcpp")
> install.packages("functional")
> install.packages("stringr")
> install.packages("reshape2")
```

安裝 rmr2，下載 [rmr2-2.2.0](https://github.com/RevolutionAnalytics/RHadoop/wiki/Downloads)，中途可能會需要你安裝 R 套件。
```
$ R CMD INSTALL rmr2_2.2.0.tar.gz
```

## 二、同 problem 2 再來計算一次 stocks 累積移動平均（CMA）
```
$ export HADOOP_HOME=/usr/java/hadoop-1.0.4/
$ ${HADOOP_HOME}/bin/hadoop fs -rmr output
$ $HADOOP_HOME/bin/hadoop fs -put test-data/stocks.txt stocks.txt
$ hadoop-book-master/src/main/R/ch8/stock_cma_rmr.R
```

遇到 argument ERROR：
```
Error in mapreduce(input = "stocks.txt", output = "output", textinputformat = rawtextinputformat,  : 
  unused argument(s) (textinputformat = rawtextinputformat, textoutputformat = kvtextoutputformat)
Execution halted
```

因為[新版 rmr2](https://github.com/RevolutionAnalytics/rmr2/blob/master/docs/getting-data-in-and-out.md) format參數 改了，請改成 `vi src/main/R/ch8/stock_cma_rmr.R` on [github](https://github.com/alexholmes/hadoop-book/blob/master/src/main/R/ch8/stock_cma_rmr.R):
```
#! /usr/bin/env Rscript
library(rmr2)
...
mapreduce(
  input = "stocks.txt",
  output = "output",
  input.format = "text",
  output.format = "text",
  map = map,
  reduce = reduce
)
```

最後，
```
# check output
$ ${HADOOP_HOME}/bin/hadoop fs -cat output/part*
# => 
AAPL	88.315
GOOG	428.875
```

![有問題的結果截圖](http://i.imgur.com/JN1SV1c.png)

發現是 map 的 input 格式出錯，更改 sample code：

```
#! /usr/bin/env Rscript
library(rmr2)

map <- function(k,v) {
  fields <- unlist(strsplit(v, ","))
  keyval(fields[1], mean(as.double(c(fields[3], fields[6]))))
}

reduce <- function(k,vv) {
  keyval(k, mean(as.numeric(unlist(vv))))
}

mapreduce(
  input = "stocks.txt",
  output = "output",
  input.format = "text",
  output.format = "text",
  map = map,
  reduce = reduce
)
```

## 問題討論
1. 為什麼只有兩筆資料？ 是 input format 錯誤??

## Reference
* [Hadoop.in.Practice.Oct.2012.pdf](http://www.manning.com/holmes/) ch8
* R [unlist](http://rfunction.com/archives/2238)
* R [Input/Output](http://www.statmethods.net/interface/io.html) 
* R [cat](http://stat.ethz.ch/R-manual/R-devel/library/base/html/cat.html)
* Nick Hsu [101-2 Progress Report](https://docs.google.com/document/d/14StHO_kcYGstR6YQU8QujTi1-pSeWx1CjCIxT7n7aW4/edit)
* rhadoop3-rhadoop-mapreduce [blog](http://cos.name/2013/04/rhadoop3-rhadoop-mapreduce/)
