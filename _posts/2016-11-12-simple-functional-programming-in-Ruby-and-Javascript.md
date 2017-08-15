---
layout: post
title:  "Simple Functional Programming in Ruby and Javascript"
date:   2016-11-12 10:23:06 +0800
comments: true
tags:
- programming
---

In Ruby, methods like `map`, `reject`, `reduce` are very handy to use. Recently, I came across Javascript functional programming, I found there are similar usages in Javascript.


### map

`map` is used to transform an array into another

Here's a simple example in Ruby, it accepts a block as parameter:

```ruby
arr = ['m', 'o', 'n', 'k', 'e', 'y']
arr.map {|x| x + '1'}
 #=> ["m1", "o1", "n1", "k1", "e1", "y1"]
```

In Javascript, `map` accepts another function as a parameter, here is an example:

```javascript
var arr = ['m', 'o', 'n', 'k', 'e', 'y'];
var arr2 = arr.map(function(x) {
  return x + '1';
});
console.log(arr2); 
// [ 'm1', 'o1', 'n1', 'k1', 'e1', 'y1' ]
```

In ES6, you can also do this with arrow function, which is much similar to Ruby way:

```javascript
var arr = ['m', 'o', 'n', 'k', 'e', 'y'];
var arr2 = arr.map((x) => x + '1');
console.log(arr2);
// [ 'm1', 'o1', 'n1', 'k1', 'e1', 'y1' ]
```

### reduce

`reduce` comes very handy to apply an operator on enumerable elements

Here is an example of `reduce` in Ruby, it takes an initial value and a block applying operation on element

```ruby
arr = [1, 2, 3, 4, 5]
arr.reduce(0) {|sum, x| sum += x}
 #=> 15
```

`reduce` can also accept a symbol as a parameter, initial value is default set to `0` for `:+`, `1` for `:*`

```ruby
arr = [1, 2, 3, 4, 5]
arr.reduce(:+)
 #=> 15
```

In Javascript, you can do this, the first parameter is a function that applies the operator, return value will be the next parameter that this function accepts, the second parameter is initial value:

```javascript
var arr = [1, 2, 3, 4, 5];
var arr2 = arr.reduce(function(sum, x) {
  return sum + x;
}, 0);
console.log(arr2);
// 15
```

In ES6, you can do this, also I prefer this way as a Ruby programmer:

```javascript
var arr = [1, 2, 3, 4, 5];
var arr2 = arr.reduce((sum, x) => sum + x, 0);
console.log(arr2);
// 15
```
<hr>

### ＼（＾∀＾）メ（＾∀＾）ノ

Let's have more fun in functional programming 

