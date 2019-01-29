---
layout: 'post'
title: Key-based cache expiration at the collection level
date: 2015/03/03
---

A common pattern in a Rails app is displaying a list of items:

{% highlight erb %}
<% @posts.each do |post| %>
  <h2><%= post.title %></h2>
  ...
<% end %>
{% endhighlight %}

You can speed up a little by caching the individual posts:

{% highlight erb %}
<% @posts.each do |post| %>
  <% cache post do %>
    <h2><%= post.title %></h2>
    ...
  <% end %>
<% end %>
{% endhighlight %}

This will cache the HTML in the `cache` block. When either the HTML or Post model changes, the cache will expire. This is called [key-based cache expiration](https://signalvnoise.com/posts/3113-how-key-based-cache-expiration-works) and is pretty sweet.

But with this scheme it's still required to fetch all posts from the database. What we'd like is a way to also cache the entire structure. To achieve that, we have to add a `cache_key` to the Post model.

{% highlight ruby %}
# concerns/cacheable.rb
module Cacheable
  extend ActiveSupport::Concern

  included do
    after_save :update_cache_key
  end

  def update_cache_key
    Redis.current.incr(self.class.redis_key)
  end

  module ClassMethods
    def cache_key
      version = Redis.current.get(redis_key)
      "#{name}/#{version}"
    end

    def redis_key
      "class-cache-key:#{name}"
    end
  end
end

# post.rb
class Post
  include Cacheable
end
{% endhighlight %}

Now we can easily cache the entire template.

{% highlight erb %}
<% cache Post do %>
  <% @posts.each do |post| %>
    <% cache post do %>
      <h2><%= post.title %></h2>
      ...
    <% end %>
  <% end %>
<% end %>
{% endhighlight %}
