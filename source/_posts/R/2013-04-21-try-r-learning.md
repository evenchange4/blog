title: try R learning
date: 2013-04-21 06:31:46
tags: [R]
--- 
> R 是一個程式語言、統計計算與繪圖的整合環境，是 [GNU](http://www.gnu.org/) 的一個project萌生於貝爾實驗室（Bell Laboratories），主要作者為 John Chambers。其語法與 S 語言（S-Plus）非常相似，提供非常多的統計工具，包含線性與非線性模型（linear and nonlinear modelling）、統計檢定（statistical tests）、時間序列分析（time series analysis）、分類分析（classification）、群集分析（clustering）等相關工具。
>
> R 擁有 ``CRAN``， a network of ftp and web servers around the world that store identical, up-to-date, versions of code and documentation for R.（目前有 4435 多種packages，幾乎由統計學家更新）

## 1. Using R
* Expressions
* Logical Values
* Variables
* Functions
* repeat print

``` r
> rep("123", times=3)
[1] "123" "123" "123"
```


<!-- more -->

* Help & Example(functionname)* 

{% codeblock lang:R %}
> help(sum)
> example(min)
{% endcodeblock %} 


* Files

{% codeblock lang:R %}
> list.files()

 [1] "Applications"    "ASUS"            "Desktop"        
 [4] "Documents"       "Downloads"       "Dropbox"        
 [7] "Google 雲端硬碟" "Library"         "Movies"         
[10] "Music"           "NTU Space"       "Pictures"       
[13] "Public"          "tmp"    

> read.csv("file.csv") # read csv file
{% endcodeblock %} 


* Run R script

{% codeblock lang:R %}
> source("file.R")
{% endcodeblock %} 


* ``na.rm`` 是否移除 NA （default = FALSE）

{% codeblock lang:R %}
> sum(a, na.rm = TRUE)
{% endcodeblock %} 



## 2. Vectors 向量
Mixing type，自動轉成 string

* ``c`` Combine 

{% codeblock lang:R %}
> c("a", TRUE, 1)
[1] "a"    "TRUE" "1"  
{% endcodeblock %} 

* Sequence 序列向量

{% codeblock lang:R %}
> 5:9
[1] 5 6 7 8 9

> seq(5,9,0.5) # 間隔 0.5
[1] 5.0 5.5 6.0 6.5 7.0 7.5 8.0 8.5 9.0
{% endcodeblock %} 

* ``[V]`` Access

{% codeblock lang:R %}
> a =c("a", TRUE, 1)
> a[3]
[1] "1"

> a[-3] # execluding
[1] "a"    "TRUE"
{% endcodeblock %} 

* ``names(v)`` 命名

{% codeblock lang:R %}
> b = 5:7
> names(b) <- c("first", "second", "third")
> b
 first second  third  
     5      6      7    
     
> b["first"]
first 
    5 
{% endcodeblock %} 

* 繪圖 Plotting

{% codeblock lang:R %}
> barplot(v) # 長條圖

> x <- seq(1,20,0.1)
> y <- sin(x)
> plot(x, y) # Scatter Plots 分散圖

{% endcodeblock %} 

* 計算

{% codeblock lang:R %}
> a = a(1,2,3)
> a+1
[1] 2 3 4
{% endcodeblock %} 

* 判別

{% codeblock lang:R %}
> a == c(1,2,5)
[1]  TRUE  TRUE FALSE
{% endcodeblock %} 

## 3. Matrice 矩陣

* 給值＋給維度

{% codeblock lang:R %}
> matrix(0,3,4)
     [,1] [,2] [,3] [,4]
[1,]    0    0    0    0
[2,]    0    0    0    0
[3,]    0    0    0    0

> matrix(1:12,3,4) # 一個一個填入
     [,1] [,2] [,3] [,4]
[1,]    1    4    7   10
[2,]    2    5    8   11
[3,]    3    6    9   12
{% endcodeblock %} 


* ``dim`` 後給維度

{% codeblock lang:R %}
> a <- 1:12
> dim(a) <- c(3,4) 
> a
     [,1] [,2] [,3] [,4]
[1,]    1    4    7   10
[2,]    2    5    8   11
[3,]    3    6    9   12
{% endcodeblock %} 

* ``[M]`` Access

{% codeblock lang:R %}
> a
     [,1] [,2] [,3] [,4]
[1,]    1    4    7   10
[2,]    2    5    8   11
[3,]    3    6    9   12
> a[2,3]
[1] 8
> a[ ,3]
[1] 7 8 9
{% endcodeblock %} 

* 繪圖 Plotting

{% codeblock lang:R %}
> contour(a)
> persp(a) # 3D
> persp(e,expand=0.2) # 擴大
> image(volcano) # 內建火山 heat map
{% endcodeblock %} 

## 4. 統計 Summary Statistics

* ``mean()`` 平均 

{% codeblock lang:R %}
> a <- 1:12
> mean(a)
[1] 6.5
> median(a)
[1] 6.5
> sd(a) # Standard Deviation
[1] 3.605551
{% endcodeblock %} 

## 5. Factors

{% codeblock lang:R %}
> a =c('gold', 'silver', 'gems', 'gold', 'gems')
> type=factor(a)> 
> type
[1] gold   silver gems   gold   gems  
Levels: gems gold silver

> as.integer(type)
[1] 2 3 1 2 1

> levels(type)
[1] "gems"   "gold"   "silver"
{% endcodeblock %} 


* 繪圖

{% codeblock lang:R %}
暫略 try R 5.2
{% endcodeblock %} 

## 6. Data Frames
* ``data.frame()`` 把同樣的 data structures 用表格呈現

{% codeblock lang:R %}
> a=c(1:3)
> b=c(2:4)
> c=c(3:5)
> f=data.frame(a,b,c)
  a b c
1 1 2 3
2 2 3 4
3 3 4 5
{% endcodeblock %} 

* Access

{% codeblock lang:R %}
> f
  a b c
1 1 2 3
2 2 3 4
3 3 4 5

> f[2]
  b
1 2
2 3
3 4

> f[[2]]
[1] 2 3 4

> f[["b"]] 
or
> f$b
[1] 2 3 4
{% endcodeblock %} 


* merge

{% codeblock lang:R %}
暫略 try R 6.4
{% endcodeblock %} 


## 7. Real-World Data

## 8. Next

slide: [Ruby & R](http://www.slideshare.net/sausheong/rubyand-r)

## Referance
# [codeschool](http://tryr.codeschool.com/)
* [blog](http://statlab.nchc.org.tw/rnotes/?page_id=2)
