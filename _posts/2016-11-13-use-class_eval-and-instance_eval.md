---
layout: post
title:  "Use class_eval and instance_eval"
date:   2016-11-13 10:23:06 +0800
comments: true
tags:
- programming
---

### current object and current class

Everything is an object in Ruby, there is an current object and current class referred to in every line of code. We use `self` to refer to current object (there is no keyword for current class).

When call a method on an object, `self`(current object) is the receiver of the method. In a class definition, `self`(current object) is the class itself.

Though there is no keyword for current class, it's easy to refer to as long as we know what is our current object.


### class_eval

`Module#class_eval` is used for modifying current class. It's commonly used when we don't know the exact name of class we want to refer to. For example,

```ruby
def add_method_to(my_class)
  my_class.class_eval do
    def foo
      puts 'foo'
    end
  end
end
			
add_method_to(String)
"bar".foo
#=> "foo"
```


One example in Rails is putting common code for several models (just like what model conern does). For example, I want to add common validations for my models, I can create a module and use `class_eval` to add methods in classes that include it. In order to get that class, we need a hook method `self.included`, and put `class_eval` part inside this method.

```ruby
module SomeCommonModule
  def self.included(base)
    base.class_eval do
      validates_presence_of :balabalabala
    end
  end
end
```

### instance_eval

`instance_eval` is used for modifying current object (self), it breaks encapsulation, be careful with it.

An example is to change instance variable of an object:

```ruby
class Foo
  def bar
    @bar = "bar"
  end
end

foo = Foo.new
foo.bar
puts foo.instance_eval { @bar }
 #=> "bar"
foo.instance_eval { @bar = "bbbbar" }
puts foo.instance_eval { @bar }
 #=> "bbbbar"
```



