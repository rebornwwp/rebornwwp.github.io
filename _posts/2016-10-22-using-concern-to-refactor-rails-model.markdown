---
layout: post
title:  "Using Concern To Refactor Rails Model"
date:   2016-10-22 15:25:09 +0800
comments: true
tags:
- programming
---

# Background #

Recently I'm working on upgrading a Rails 2.3 application to Rails 4.2 (Yes, it's true, our company is still using 2.3 now, ( 'Θ')ﾉ( 'Θ')ﾉ( 'Θ')ﾉ). There are 5 models containing codes which are 90% the same. I tried to refactor them with Rails 4's concern module.
<hr>
# What is concern #

### Rails 2.3 way
In Rails 2.3, if you extract code from model to a module, you need to create a module and put it into your `/lib` directory. To realize validations and class methods, you need to add a hook method in it, which was not an elegant way.


```ruby
module Taggable
  def  self.included(base)
    base.extend ClassMethods
    base.class_eval do
      scope :disabled, -> { where(disabled: true) }
      # scope, validations, after_save .....
    end
  end

  module ClassMethods
    def find_with_tag(tag)
	 # .....
    end
  end
end
```


### Realization with concern
In Rails 4, you can eaily realize it by creating a module extending `ActiveSupport::Concern` and include this module in your model.


```ruby
module Taggable
  extend ActiveSupport::Concern
  included do
    scope :disabled, -> { where(disabled: true) }
  end
  
  class_methods do
    def find_with_tag(tag)
	 # ...
    end
  end
  
  #put instance methods here
end
```

### Notice
- If you want to know which class is using this module in a class method, you can simply call `self`. For example, I want to cache class method `find`, then in my taggble module, I use

```ruby
class_methods do
  def some_class_method(id)
    Rails.cache.fetch("#{self.name}.find(#{id})", :expires_in => 15.minutes.to_i)
  end
end
```

- If it's in an instance method, use `self.class` instead

```ruby
def some_instance_method(id)
  Rails.cache.fetch("#{self.class.name}.find(#{id})", :expires_in => 15.minutes.to_i)
end
```

And the code is more elegant (^m^ )クスッ.
