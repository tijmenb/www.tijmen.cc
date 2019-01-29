---
layout: 'post'
title: "Handling changing validations in ActiveRecord models"
---

During the lifetime of a Rails app, validations can change. By changing validations on existing models, you run the risk of invalidating records that were okay before.

This can have awkward consequences, for example when you add a `validates_presence_of :description` for a `Post` model and there are existing records without that `:description`.

Any later update of that `Post` will raise an error (or silently fail), and not where you would expect it to.

What to do about this?

### 1. Use form objects

Using form objects at the right places will prevent you from having to add too much validations on your model in the first place. Add validations for **data you want** to form objects, and add **validations you need** to models.

Take the Post model as example:

- `validates_presence_of :title` belongs in the Model, if the title is needed to compose the URL slug.
- `validates_presence_of :seo_description` belongs in form object. The application doesn't crash without SEO text, but you might want to make it mandatory for the user to provide.

### 2. Don't forget the database

By getting into the habit of adding `NOT NULL` constraints to your database as well as validation, you will be warned of problems a lot sooner.

{% highlight ruby %}
change_column :posts, :title, :string, null: false
{% endhighlight %}

### 3. Check (a sample) each deploy

After deploying, you could run a Rake task checking every record.

{% highlight ruby %}
task :verify_records do
  ActiveRecord::Base.subclasses.each do |klass|
    klass.find_each do |record|
      next if record.valid?
      Rails.logger.error "#{klass}.find(#{record.id}) # is invalid."
    end
  end
end
{% endhighlight %}



#### Related

- [The Perils of Uniqueness Validations](https://robots.thoughtbot.com/the-perils-of-uniqueness-validations)
