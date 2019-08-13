---
title: "LRUG: Reuse your government's code"
date: 2019/08/13
layout: post
redirect_from: "/lrug/"
---

I gave a talk at LRUG yesterday about [reusing code from GOV.UK](https://skillsmatter.com/skillscasts/14335-reuse-your-government-s-code). I'll put up the slides here soon. In the meantime, these are the GOV.UK projects mentioned in the talk:

## 3 real world applications

- [Whitehall](https://github.com/alphagov/whitehall), a really big app with a lot of history
- [Content Data API](https://github.com/alphagov/content-data-api), a data warehouse that uses a star schema database
- [Content Publisher](https://github.com/alphagov/content-publisher), a new app structured in novel ways

## 3 cool patterns

- Readable feature specs, as [shown in Content Publisher](https://github.com/alphagov/content-publisher/tree/master/spec/features)
- A spam honey pot, as [shown in the feedback application](https://github.com/alphagov/feedback/search?q=giraffe&unscoped_q=giraffe)
- A way to archive big tables, as [shown in email-alert-api](https://github.com/alphagov/email-alert-api/pull/627/files)

## 3 things to help big projects

- We configure lots of repos with a [script in govuk-saas-config](https://github.com/alphagov/govuk-saas-config/tree/master/github)
- We share frontend code using components in [the govuk_publishing_components gem](https://github.com/alphagov/govuk_publishing_components)
- We do some visual regression testing using [govuk-visual-regression](https://github.com/alphagov/govuk-visual-regression)

## 3 clever team tools

- [Seal of Approval](https://github.com/binaryberry/seal) will remind you to review PRs
- [GitHub Trello Poster](https://github.com/emmabeynon/github-trello-poster) will post your PR to Trello tickets
- [GOV.UK browser extension](https://github.com/alphagov/govuk-browser-extension) allows you to switch between environments  
