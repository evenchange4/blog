title: Ruby bits (block 篇)
date: 2013-04-23 05:51:11
tags: [ruby]
---

## 目錄 (block、Proc、LAMBDA 篇)
1. YIELD
2. Proc
3. LAMBDA

*當 block 之後還需要被使用：Proc、LAMBDA*
<!-- more -->

## 1. YIELD（丟 block 進去）
```
def call_this_block_twice
  yield
  yield 
end
```
```
call_this_block_twice { puts "twitter" }
=> 
twitter
twitter
```

### ￼YIELD - ARGUMENTS
```
def call_this_block
  yield "tweet"
end
```
```
call_this_block { |myarg| puts myarg.upcase }
=> TWEET
```

### ￼YIELD - RETURN VALUE
```
def puts_this_block
  puts yield
end
```
```
puts_this_block { "tweet" }
=> tweet
```

### ￼YIELD - 參數
```
def yield_name(name)
  yield name
end
```
```
yield_name("Eric") { |e| puts "My name is #{e}." }
=> My name is Eric.
```

### include ``Enumerable``（路邊撿到拿來用）
```
class Timeline
  def each
  ...
  end
  include Enumerable
end
```
```
timeline.sort_by  { |tweet| tweet.created_at }
timeline.map      { |tweet| tweet.status }
timeline.find_all { |tweet| tweet.status =~ /\@codeschool/ }
```

## 2. Proc ``Proc.new {block} `` ``.call``或``&``
```
my_proc = Proc.new { puts "tweet" } 
my_proc.call # => tweet
```

```
floats = [1.2, 3.45, 0.91, 7.727, 11.42, 482.911]

round_down = Proc.new do |e|
    e.to_i
end

ints = floats.collect(&round_down)
=> [1, 3, 0, 7, 11, 482]
```

## 3. LAMBDA  `` lambda {block} `` ``.call``
```r
my_proc = lambda { puts "tweet" } 
my_proc.call  # => tweet
```

### multiple LAMBDAS（當作參數丟進去.call，而不用 yield）
```
class Tweet
  def post(success, error)
￼   if authenticate?(@user, @password)
      success.call
    else
      error.call
    end 
  end
end
```
```
tweet = Tweet.new('Ruby Bits!')
success = -> { puts "Sent!" }
error = -> { raise 'Auth Error' }

tweet.post(success, error)
```

### ￼``&`` LAMBDA 返回 BLOCK （當原本需要的還是一個 block）
*原本*
```ruby 
tweets = ["First tweet", "Second tweet"] 
tweets.each do |tweet|
  puts tweet
end
```

*使用 lambda* 
```ruby 
tweets = ["First tweet", "Second tweet"]
printer = lambda { |tweet| puts tweet }
tweets.each(&printer)
```

### 互轉 Block -> proc -> block
```ruby 
class Timeline
  attr_accessor :tweets
  def each(&block)       # (define) Block into proc
    tweets.each(&block)  # (calling)proc back into a Block
  end 
end
```

### ``&#.to_proc`` as SYMBOL#TO_PROC 、 ``:`` as pretzel colon
```
[[1,'a'],[2,'b'],[3,'c']].map { |e| e.first }
等價於
[[1,'a'],[2,'b'],[3,'c']].map(&:first.to_proc)
等價於 縮寫後 
[[1,'a'],[2,'b'],[3,'c']].map(&:first)
#=> [1, 2, 3]
```

```ruby 
[1,2,3,4,5].inject{|sum, i| sum += i}
等價於 縮寫後 
[1,2,3,4,5].inject(&:+)
#=> 15
```

## Reference
* http://stackoverflow.com/questions/1217088/what-does-mapname-mean-in-ruby
* http://stackoverflow.com/questions/11856978/reference-to-a-method




