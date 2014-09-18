title: Ruby bits (method 篇)
date: 2013-04-23 04:24:47
tags: [ruby]
---

## 目錄 (method 篇)
1. OPTIONAL ARGUMENTS
2. HASH ARGUMENTS
3. “SPLAT” ARGUMENTS

<!-- more -->

## 1. OPTIONAL ARGUMENTS （給予參數初始值）
``` Ruby 
# bad
def tweet(message, lat, long)
  ...
end
tweet("Practicing Ruby-Fu!", nil, nil)

# good
deftweet(message,lat=nil long=nil)
  ...
end
tweet("Practicing Ruby-Fu!")
```

## 2. HASH ARGUMENTS（參數太長）
``` Ruby
def tweet(message, options = {})
  status = Status.new
  status.lat = options[:lat]
  status.long = options[:long]
  status.body = message
  status.reply_id = options[:reply_id]
  status.post
end
```
``` ruby
tweet("Practicing Ruby-Fu!",
  :lat => 28.55,
  :long => -81.33,
  :reply_id => 227946
)
```

### better
``` ruby
tweet("Practicing Ruby-Fu!",
  lat: 28.55,
  long: -81.33,
  reply_id: 227946
)
```

### Keys are optional
``` ruby
tweet("Practicing Ruby-Fu!",
  lat: 28.55,
)
```

### Complete hash is optional
``` Ruby
tweet("Practicing Ruby-Fu!")
```

##￼3. “SPLAT” ARGUMENTS
``` Ruby
def mention(status, *names)
  tweet("#{names.join(' ')} #{status}")
end

mention('Your courses rocked!', 'eallam', 'greggpollack', 'jasonvanlue')
=> eallam greggpollack jasonvanlue Your courses rocked!
```
