---
layout: post
title: The GOV.UK docs review system
date: 2017/06/11
---

The hardest thing about documentation is keeping it up to date. One thing that helps is having a formal system to make sure you regularly review the content.

At GOV.UK we have given [all our docs pages][docs] a "review-by date". After this date the content must be reviewed.

## Configuring the review date

Each page has 3 variables: the date it was last reviewed, who needs to review it, and how soon the page should be reviewed again. It's configured in the [middleman front matter][fm] of the page.

![](/media/2017-06-11-keeping-docs-up-to-date-1.png)

Pages that are likely to change soon will typically have review dates of a month or so. Others are more static and are set to 6 months or even longer.

## Showing review dates

Every page has the owner and calculated review date in the footer of the page:

![](/media/2017-06-11-keeping-docs-up-to-date-4.png)

Pages that need to be now are displayed on the [docs homepage][docs]. We also show the pages that will need review in the next week.

![](/media/2017-06-11-keeping-docs-up-to-date-2.png)

Once a page is expired it will show a prominent warning. This means the reader will see that it may no longer be current.

![](/media/2017-06-11-keeping-docs-up-to-date-3.png)

## Slack

Each page also has owner. This can either be a Slack channel or a Slack username. Once the page expires, the owner will be notified in a message. This is done by our [documentation robot][robot], which is currently named "Daniel the Manual Spaniel" for unclear reasons. It runs every weekday at 11:30.

![](/media/2017-06-11-keeping-docs-up-to-date-5.png)

The notification will keep repeating until the page is fixed.

## Further reading

Here are some useful links:

- [GOV.UK Developer Documentation][docs]
- [Original PR that implemented the feature](https://github.com/alphagov/govuk-developer-docs/pull/102)
- [PR to add review dates retroactively](https://github.com/alphagov/govuk-developer-docs/pull/123)
- [GOV.UK Docs Monitor][robot]

<style>
img { border: 1px solid #333 }
</style>

[robot]: https://github.com/tijmenb/govuk-docs-monitor
[fm]: https://jekyllrb.com/docs/frontmatter/
[docs]: https://docs.publishing.service.gov.uk/manual.html
