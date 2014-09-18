title: Ruby bits (module 篇)
date: 2013-04-23 04:24:47
tags: [ruby]
---

## 目錄 (module 篇)
>modules as being very much like classes, only modules can't create instances and can't have subclasses. They're just used to store things!

1. NAMESPACE
2. MIXIN

<!-- more -->

## 1. NAMESPACE（避免同樣的 method name）
*image_utils.rb*
```
module ImageUtils
  def self.preview(image)
  end

  def self.transfer(image, destination) 
  end
end
```
*run.rb*
```
require 'image_utils'
image = user.image
ImageUtils.preview(image)
```

* example 2

```
module Math
  PI = 3.141592653589793
end

puts Math::PI #=> 3.141592653589793
```

##￼2. MIXIN
### ``include``: 可直接當作 ``instance method`` 來 include
*IMAGE_UTILS.RB*
```
module ImageUtils
  def preview
  end
  def transfer(destination) 
  end
end
```

*AVATAR.RB*
```
require 'image_utils'
class Image
  include ImageUtils
end
```

*RUN.RB*
```
image = user.image
image.preview
```

### ￼MIXINS（像是多重繼承） VS CLASS INHERITANCE（只能單一superclass）
```
class Post
  include Shareable
  include Favoritable
end

class Image
  include Shareable
  include Favoritable
end

class Tweet
  include Shareable
  include Favoritable
end
```
```
module Shareable
  def share_on_facebook
  end
end
￼
module Favoritable
  def add_to_delicious
  end
end
```

### ``extend``: 可直接當作 ``class method`` 來 extend
*HOOKS*
```
module ImageUtils
  def preview
  end

  def transfer(destination)
  end

  module ClassMethods
    def fetch_from_twitter(user)
    end 
  end

end
```
```
class Image
  include ImageUtils
  extend ImageUtils::ClassMethods
end
```
```
image = user.image
image.preview                      # instance method
Image.fetch_from_twitter('gregg')  # class method
```

￼