---
layout: post
title: Measure Rails' cache hit ratio in statsd
date: 2017/03/14
---

A good way to improve the performance of your application is caching. If you're
unsure whether your caching strategy is working it's a good idea to start measuring
cache hits and cache misses.

Assuming you have statsd set up, you can subscribe to [ActiveSupport notifications][as-not] and
pass down the metrics. This is what you can do in an initialiser:

```ruby
# config/initializers/instrumentation.rb
statsd = Statsd.new

ActiveSupport::Notifications.subscribe("cache_read.active_support") do |_name, _start, _finish, _id, payload|
  hit_or_miss = payload[:hit] ? "cache_hit" : "cache_miss"
  statsd.increment("cache.#{hit_or_miss}", 1)
end
```

You will be able to test this by listening to UDP port `8125` (statsd) in another
terminal. If you excercise the code you'll see something like this:

```sh
$ nc -u -l 8125
cache.cache_hit:1|c
```

That means it's working!

[as-not]: http://guides.rubyonrails.org/active_support_instrumentation.html
