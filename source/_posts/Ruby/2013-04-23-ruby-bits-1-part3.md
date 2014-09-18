title: Ruby bits (class 篇)
date: 2013-04-23 04:24:47
tags: [ruby]
---

## 目錄 (class 篇)

1. Scope
2. attr_accessor、attr_reader、self
3. private、protected
4. Inheritance
5. ACTIVESUPPORT
6. Struct
7. Send
8. method

<!-- more -->

## 1. Scope ``$: global``、``@@: class``、``@: instance``
```
class Computer 
  $manufacturer = "Mango Computer, Inc."  # global variable
  @@files = {hello: "Hello, world!"}      # class variable
  
  def initialize(username, password)
    @username = username                  # instance variable
    @password = password
  end
  
  def current_user
    @username
  end
  
  def self.display_files                  # class method
    @@files
  end
end
```
```
hal = Computer.new("Dave", 12345)

puts "Current user: #{hal.current_user}" #=> Current user: Dave
puts "Manufacturer: #{$manufacturer}"    #=> Manufacturer: Mango Computer, Inc.
puts "Files: #{Computer.display_files}"  #=> Files: {:hello=>"Hello, world!"}
```


## 2. attr_accessor (getter & setter)
``` Ruby
attr_accessor :baz
```

### same as
``` Ruby
def baz=(value)
  @baz = value
end

def baz
  @baz
end
```

### attr_reader、attr_writer、self
``` Ruby
class Tweet
  attr_accessor :status
  attr_reader :created_at   # 沒有setter
  def initialize(status)
    self.status = status    # 或 @status = status
    @created_at = Time.new  # 不能 self.created_at
  end
end
```

*``self``: the current object，需要 setter。  *

```
class Person
  attr_reader :name           # getting
  attr_writer :job            # changing
  def initialize(name, job)
    @name = name
    @job = job
  end
end
```

* attr_reader + attr_writer = attr_accessor


## 3. private（cannot be called with explicit receiver）
* 預設是 public

``` Ruby
class User
  def up_vote(friend)
    bump_karma         # 能呼叫
    friend.bump_karma  # 不能呼叫（外來物件）
  end

  private
  def bump_karma
    puts "karma up for #{name}"
  end

end
```

### protected （可被 class 內呼叫）
``` Ruby
class User
  def up_vote(friend)
    bump_karma         # 能呼叫
    friend.bump_karma  # 能呼叫
  end

  protected
  def bump_karma
    puts "karma up for #{name}"
  end

end
```

## 4. Inheritance（ Override、``super``）
```r
class Creature
  def initialize(name)
    @name = name
  end
  
  def fight
    return "B"
  end
end

class Dragon < Creature
  def fight                   # Override
    puts "A" 
    super                     # super         
  end  
end
```
```
=> 
A
B
```

## 5. ACTIVESUPPORT
```
# install
$ gem install activesupport
$ gem install i18n
￼
# load it
require 'active_support/all'
```

## 6. Struct
```ruby 
class Tweet
  attr_accessor :user, :status
  def initialize(user, status)
    @user, @status = user, status
  end 
  def to_s
    "#{user}: #{status}"
  end
end
```

*等價於*
```ruby
Tweet = Struct.new(:user, :status) do 
  def to_s
    "#{user}: #{status}"
  end
end
```

## 7. Send（可以呼叫 private or protected method）
```
class Timeline
  def initialize(tweets)
    @tweets = tweets
  end
  def contents
    @tweets
  end 

  private
  def direct_messages
  end 
end
```
```
tweets = ['Compiling!', 'Bundling...']
timeline = Timeline.new(tweets)

timeline.contents
等價於
timeline.send(:contents)
等價於
timeline.send("contents")

timeline.send(:direct_messages)  # call private
```

## 8. method （放在記憶體，等等再 ``.call`` ）
```
class Timeline
  def initialize(tweets)
    @tweets = tweets
  end
  def contents
    @tweets
  end
end
```

```
content_method = timeline.method(:contents)
#=> #<Method: Timeline#contents>
content_method.call
#=> ["Compiling!", "Bundling..."]
```


