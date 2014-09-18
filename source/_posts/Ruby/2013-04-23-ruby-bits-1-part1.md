title: Ruby bits 
date: 2013-04-23 03:43:06
tags: [ruby]
permalink: ruby-bits-1-part1
---
## 目錄
1. if、Unless
2. nil 
3. 給予初始值
4. RETURN VALUES
5. symbol

<!-- more -->

## 1. Unless 更直覺
```ruby 
# bad
if ! tweets.empty?
  puts "Timeline:"
  puts tweets
end
￼￼
# good
unless tweets.empty?
  puts "Timeline:"
  puts tweets
end
```

### 一行表達 if
```
puts pin_number == pin ? "Balance: $#{@balance}." : pin_error
```
```
if pin_number == pin
  puts "Balance: $#{@balance}."
else
  puts pin_error
end
```





## 2. nil treated as ``false``
``` ruby 
# bad
if attachment.file_path != nil
  attachment.post
end

# good
if attachment.file_path
  attachment.post
end
```

##￼3. 給予初始值 - “OR”
``` ruby
tweets = timeline.tweets || []
```

``` ruby
result = nil || 1      # =>1
result =   1 || nil    # =>1
result =   1 || 2      # =>1
```

``` ruby
result2 = nil || 1     # =>1
等價於
result2 ||= 1          # =>1
```

*但是 if else 比較易讀*

## 4. RETURN VALUES
``` ruby
# bad
def list_url(user_name, list_name)
  if list_name
    url = "https://twitter.com/#{user_name}/#{list_name}" 
  else
    url = "https://twitter.com/#{user_name}" 
  end
  url
end

# good
def list_url(user_name, list_name)
  if list_name
    "https://twitter.com/#{user_name}/#{list_name}"
  else
    "https://twitter.com/#{user_name}"
  end 
end
```

## 5. Symbol
string 與 symbol 轉換

```
"a".to_sym
=> :a

"a".intern
=> :a

:a.to_s
=> "a"
```
```
:hello.is_a? Symbol
=> true
```
