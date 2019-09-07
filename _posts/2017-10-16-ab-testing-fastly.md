---
layout: post
title: Using Fastly for A/B testing
---

Since last year we're running A/B tests on GOV.UK using Fastly, our CDN.

## A/B testing

Most A/B testing usually takes place using some kind of server-side implementation.

When the user requests the page, the server will place the user in the `A` or `B` bucket and serve them the appropriate version. To make sure that the user will see the same version the next time a cookie is placed.

Examples of this method for Rails are [Split](https://github.com/splitrb/split) and [Vanity](https://github.com/assaf/vanity).

## A/B with a CDN

On GOV.UK this system won’t work. To make the site fast and always available to users, we’re using [Fastly](https://www.fastly.com/) a Content Delivery Network (CDN). This means that most requests to www.gov.uk are served from a cache rather than our own servers.

Having most pages cached makes the site super fast, but it makes a straightforward server side implementation impossible. Without any extra configuration, you would end of caching the A or B version, and serving that for however long your cache TTL is.

However, it's possible to move the A/B testing to the CDN level. This is how that works:

- Fastly determines if the user sees the A or the B variant
- Instead of a cookie, Fastly sends the cookie variant to origin with a HTTP header
- The application reads this header and responds with the chosen variant, as well as a `Vary` HTTP header
- Because of the `Vary` header, Fastly uses a separate cache for each variant
- Finally, Fastly sets a cookie so that it can show the same version next time

## Configuring Fastly

To get what we wanted meant we had to build the A/B testing code to work with the CDN. This has 3 parts: we have to configure cookies, determine the correct page to show and get the correct page cached by the CDN.

Our CDN is configured using the Varnish Configuration Language (VCL). The following is the VCL to configure A/B tests, simplified a bit.

```pl
sub vcl_recv {
  if (req.http.Cookie ~ "ABTest-Example") {
    set req.http.MyABTest =  req.http.Cookie:ABTest-Example;
  } else {
    if (randombool(5,10)) {
      set req.http.MyABTest = "B";
    } else {
      set req.http.MyABTest = "A";
    }
  }
}

sub vcl_deliver {
  add resp.http.Set-Cookie = "ABTest-Example=" req.http.MyABTest;
}
```

Let's look at it in detail:

### Use the cookie if set

First, we check if the user already has a cookie. If so, use the value of the
cookie in the HTTP header.

```pl
if (req.http.Cookie ~ "ABTest-Example") {
  set req.http.MyABTest = req.http.Cookie:ABTest-Example;
}
```

### Pick a random bucket otherwise

The first step is to get people into the correct bucket:

```pl
if (randombool(5,10)) {
  set req.http.MyABTest = "B";
} else {
  set req.http.MyABTest = "A";
}
```

This `randombool(5,10)` function in VCL will return true 50% of the time.

This makes Varnish send a HTTP header called `MyABTest` with the variant to your server. This means we can switch the template in the application.

### Serve a variant

In your application you can now change the variant on the basis of the HTTP header:

```rb
if request.headers["MyABTest"] == "B"
  render "b_template"
else
  render "default_template"
end
```

### Caching behaviour

But at this point we would still have a problem: Varnish would cache the page once, no matter if A or B was activated.

To counter this, we use the `HTTP Vary` header. `Vary` is a clever header that basically creates a cache per thing. For example, setting `Vary: User-Agent` will make the cache save copies per use agent, so that we save a different version depending on the user’s browser.

We can use this to create a cache for each bucket:

```rb
# application code
response_headers["Vary"] = "MyABTest"
```

### Cookies

Then finally, we’re going to have to make sure that we set a cookie for the user. In VCL:

```pl
add resp.http.Set-Cookie = "ABTest-Example=" req.http.MyABTest;
```

This will set a cookie with the variant the user has seen, so that the next time we can show the same version to the user.

## More reading

- [GOV.UK documentation on A/B tests](https://docs.publishing.service.gov.uk/manual/ab-testing.html)
- [A/B testing at the edge](https://www.fastly.com/blog/ab-testing-edge)
