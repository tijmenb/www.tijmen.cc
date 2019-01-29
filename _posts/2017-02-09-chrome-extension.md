---
layout: post
title: How we built a browser extension for GOV.UK developers
date: 2017/02/09
---

Last year we started building a [Chrome extension for developers on GOV.UK][gh].

GOV.UK is a pretty big project. We have many applications, internal tools and API endpoints. This means the project can be daunting to navigate for developers, especially when they’re new. On Slack, we would often see questions like “what’s the URL for staging?” and “how do I see what app is responsible for rendering this page?”.

Traditionally we used to use a collection of Javascript bookmarklets to help with this. A downside to those is that they become out of date quickly and there’s no easy way to update. They’re also hard to test. As an experiment we ported some of the bookmarklets to a Chrome extension. This turned out to work well, and we’ve expanded on it ever since. I’ll show you what it can do.

## Switching between environments

![](/media/2017-02-05-chrome-extension-1.gif)

We have three environments: production (www.gov.uk), a staging environment to verify our deploys and an integration environment to test new features. Developers also have their own development environment. The extension allows you to quickly switch between these, which makes it really easy to check the differences after a change.

## Switching between content metadata

![](/media/2017-02-05-chrome-extension-2.gif)

GOV.UK exposes a lot of metadata about the content on the site. For example, [/info pages][info] show basic statistics and user needs for most pages. Most of this data can be accessed by changing the URL. For example, the info page for [gov.uk/apply-uk-visa](https://www.gov.uk/apply-uk-visa) lives at [gov.uk/info/apply-uk-visa](https://www.gov.uk/info/apply-uk-visa).

The extension helps here too. For example, allows you to quickly switch between: info pages, the raw search data, data from our content APIs, draft content and earlier versions from the National Archives. It even links directly to our user feedback system, so you quickly check what users have been saying about a particular piece of content.

## External links

![](/media/2017-02-05-chrome-extension-3.gif)

Finally, using the metadata we can link to locations are useful to developers and editors. The extension show what app is responsible for displaying the content to users and which app has published the content.

If you’re interested in building a similar extension for your organisation, you can [find the source code on GitHub][gh].

[info]: https://insidegovuk.blog.gov.uk/2014/10/29/info-pages-publishing-data-about-user-needs-and-metrics/
[gh]: https://github.com/alphagov/govuk-toolkit-chrome
