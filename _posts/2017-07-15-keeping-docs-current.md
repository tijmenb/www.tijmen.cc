---
layout: post
title: 5 ways to keep internal tech documentation up to date
date: 2017/07/05
---

Over the last year we've been improving the developer documentation on GOV.UK. The goal of this is to make it easier for developers to do their job.

## 1. Make it easy to find

There should be only one place to find documentation. Internally we use the `publishing.service.gov.uk` domain, so we decided on using [docs.publishing.service.gov.uk][docs] for the internal docs. The site is publicly accessible, because [making things open makes them better][open]. Making the docs open means that it's easy to find on Google as well.

There are a few other entry points. The [GOV.UK chrome extension][ext] links to the [content schemas][schemas], [document types][doctypes] and [application pages][apps]. Every [repository on GitHub][repos] links to the docs.

[open]: https://www.gov.uk/design-principles#tenth
[docs]: https://docs.publishing.service.gov.uk
[ext]: /2017/02/09/chrome-extension.html
[schemas]: https://docs.publishing.service.gov.uk/content-schemas.html
[doctypes]: https://docs.publishing.service.gov.uk/document-types.html
[apps]: https://docs.publishing.service.gov.uk/apps.html
[repos]: https://github.com/search?q=topic%3Agovuk+org%3Aalphagov

## 2. Keep docs close to code

The closer the code is to the documentation the more likely it is a developer will update it. Of course, this conflicts with the previous principle of having a single place for everything.

We deal with this by pulling in the docs from GitHub. For example: the [content-store docs][cs] are copied from [GitHub directly][cs-src].

[cs]: https://docs.publishing.service.gov.uk/apis/content-store.html
[cs-src]: https://github.com/alphagov/govuk-developer-docs/blob/master/source/apis/content-store.html.md.erb

## 3. Automate docs generation

Docs shouldn't only be prose. Because all GOV.UK repos are open, we have the opportunity to integrate code, data and configs into the docs.

For example: the [page that describes all fields in the search API][sapi] is constructed by fetching the [definitions that are used in the application][rummager-src]. This means it never goes out of date.

Another example are [application pages such as this][apps]. They [use the GitHub API][gh-api] to fetch the repository description and tags.

[sapi]: https://docs.publishing.service.gov.uk/apis/search/fields.html
[rummager-src]: https://github.com/alphagov/rummager/blob/master/config/schema/field_definitions.json
[apps]: https://docs.publishing.service.gov.uk/apps/content-store.html
[gh-api]: https://github.com/alphagov/govuk-developer-docs/blob/master/app/app_docs.rb

## 4. Make it feel good to update

Updating docs isn't the most fun thing. To create a sense of achievement, it helps if the docs themselves look nice.

We're lucky to have the [tech-docs-template][], which provides a well designed, responsive, accessible, user researched template for tech docs. You can [read more about it on the GDS blog][tech-docs-blog].

[tech-docs-template]: https://github.com/alphagov/tech-docs-template
[tech-docs-blog]: https://gds.blog.gov.uk/2017/03/29/introducing-our-new-product-pages-and-technical-documentation/

## 5. Have a formal review system

Implement a system that forces you to keep it up to date.

On GOV.UK each page has a "review by" date. For more on that, [continue reading about the review system][rev] in a previous post.

[rev]: /2017/06/11/docs-review-system.html
