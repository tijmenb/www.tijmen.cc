---
layout: post
title: How we use random content at GOV.UK
date: 2016/10/01
---

On GOV.UK we recently started using our JSON schemas to generate random test data. It has uncovered a number of potential bugs and increased confidence in the system.

### Testing a distributed system

The new GOV.UK publishing platform is built with many publishing applications that allow editors to create and edit specific types of pages. The data is sent to a [central publishing API](https://github.com/alphagov/publishing-api). The frontend applications use a read-only version of this API to fetch data and display it to the user.

For example, [pages like this accident report](https://www.gov.uk/aaib-reports/aaib-investigation-to-eurofox-912-s-g-chup-31-march-2016) are edited by departments using an application called [Specialist Publisher](https://github.com/alphagov/specialist-publisher-rebuild). The actual page is then served by [Specialist Frontend](https://github.com/alphagov/specialist-frontend) using JSON from the [content-store](https://github.com/alphagov/content-store).

This kind of architecture has an inherent danger: it assumes that the publishing applications will produce JSON that the frontend applications can understand.

To test our applications we employ a technique called contract testing. [Daniel Roseman wrote about this last year on the GDS technology blog](https://gdstechnology.blog.gov.uk/2015/01/07/validating-a-distributed-architecture-with-json-schema/). In short, we use [JSON Schema](http://json-schema.org/) to coordinate changes between apps.

### Random data

We've recently started using our JSON schemas to generate random data. Because the JSON schemas are so structured, we are able to reliably generate valid test data. We've [made a little gem](https://github.com/alphagov/govuk_schemas) that given a schema outputs a blob of JSON that is guaranteed to be valid, but filled with random data.

For example, given this schema:

```json
{
  "required": ["page_size"],
  "properties": {
    "title": {
       "type": "string"
     },
    "page_size": {
      "type": "integer"
     }
  }
}
```

Our random example generator will produce this:

```shell
> RandomContent.generate(schema)
#=> { "title" => "Lorem Ipsum", "page_size" => 1 }
> RandomContent.generate(schema)
#=> { "page_size" => 10 }
> RandomContent.generate(schema)
#=> { "title" => "Ipsum Lorem", "page_size" => 21 }
> RandomContent.generate(schema)
#=> { "title" => "Dolor Lorem", "page_size" => 0 }
```

While it's still young, it's helping us tremendously in development.

### Use case 1: finding potential bugs in frontend apps

Say we have this innocuous looking code in the consumer:

```ruby
class PageController < ApplicationController
  def show
    content = api.get_content("/test-page")
    @number_of_pages = results / content["page_size"]
  end
end
```

You probably see the potential for a bug. The schema doesn't enforce that the page size is higher than 0, so the code is at danger of

```
ZeroDivisionError: divided by 0
```

The problem is that these kinds of potential errors are increasingly hard to spot if you're dealing with distributed systems. So how do we find what we're actually relying on, instead of what we think we rely on?

```ruby
100.times do
  it "expects to render with a valid payload"
    random_data = RandomContent.generate(schema)
    stub_api_request(random_data)

    get "/test-page"

    expect(response.code).to eql(200)
  end
end
```

If we run the randomised data generator often enough, we'll encounter a case where `page_size` is zero and we expose our bug.

### Use case 2: realistic unit testing

The second use case is to counter a common bug. Let's say you have this method:

```ruby
def do_something_with_api_response(api_response)
  api_response["title"].upcase
end
```

Being a sensible developer you test the method:

```ruby
it "upcases the title" do
  item = {
    "title" => "Foo"
  }

  result = do_something_with_api_response(item)

  expect(result).to eql("FOO")
end
```

This is excellent so long as `item` is really what the API returns.

```ruby
item = Random.merge_and_validate({
  "title" => "Foo"
})
```

Will generate a random item and merge it our `title` and then validate it against the schema. This will make us 100% confident that the API will actually send `title` in the response.

### Use case 3: pages with random content

Our frontend applications can work with random data. We created [a little Sinatra app](https://github.com/alphagov/govuk-random-content-store) that serves random content. It's [hosted on Heroku](https://govuk-random-content-store.herokuapp.com/content/detailed_guide).

It results in pages like this:

![An example of a page rendered with random content](/media/2016-10-01-random-data-schema-testing-1.png)

This is quick way for our designers to see what content potentially breaks the layout of the page.
