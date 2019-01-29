---
layout: post
title:  "Sort a hash by value in Ruby"
date: 2015-09-16
---

If you're like me and always forget things, here's how you sort a hash by its value in Ruby:

```ruby
hash = { foo: 2, bar: 3, zed: 1 }

def sort_hash_by_value(hash)
  Hash[hash.sort_by(&:last).reverse]
end

sort_hash_by_value(hash)
=> { bar: 3, foo: 2, zed: 1 }
```
