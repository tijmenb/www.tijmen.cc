---
layout: post
title: How robots help keep GOV.UK up to date
date: 2018/10/02
---

The first commit to GOV.UK is almost 8 years old and we currently run around 60 applications. Over the last 6 months, we’ve used an automated tool to keep our applications up to date.

In 2017, we wrote a [little tool to analyse and visualise dependencies](https://github.com/alphagov/govuk-dependency-analysis) across our applications. One of the things we did was generate a treemap to show all dependencies and their versions. This showed we were using 487 different dependencies, in 1179 different versions - for example, we were running 13 distinct versions of Rails.

![](/media/2018-10-02-1.png)

A lot of these dependencies were running in different versions. For example, the diagram above shows that we were using 13 different versions of Ruby on Rails, and 9 different versions of Unicorn (our web server).

Updating dependencies becomes more difficult over time. An application with many outdated dependencies make updating complicated. This is because a lot of code will depend on specific dependency versions, which may even depend on other outdated dependencies. It’s like moving house by dragging out all bookcases with books. It’s possible but really tiring and slow.

Our team needed to find a way to consolidate our dependencies so we could:

- follow the [GOV.UK policy](https://docs.publishing.service.gov.uk/manual/keeping-software-current.html) of keeping our dependencies up to date
- react quickly to security updates
- save developers time and frustration by letting them use the same version of dependencies

## Choosing a tool to help us update dependencies

We decided on a dual approach to fix our problem. Our first priority was to get all our applications on the latest version of Ruby on Rails. We also wanted to create a system that would save us from taking a big-bang approach.

We decided to investigate a number of tools that do automatic dependency updating. These tools all work in a similar way by keeping track of the dependencies of an application. When a new version of the dependency appears, a Pull Request is raised against the repository. Tools will often add changelog information in the Pull Request.

![](/media/2018-10-02-3.png)

After evaluating Snyk, Depfu and Dependabot, we decided on Dependabot, because it's [partially open source](https://github.com/dependabot/dependabot-core).

The first stage was a lot of hard work. We estimated Dependabot made around 2,000 Pull Requests to get everything up to the latest version.

## Benefits we're seeing

### 1. Everything is more up to date

In total, we’ve decreased the number of different dependency versions we’re running by a quarter, even if we’ve increased the actual number of applications:

In September 2017 our applications had a combined total of 1179 different gem versions. In September 2018 this had decreased to 871 different versions, a -26% decrease.

![](/media/2018-10-02-2.png)

### 2. We’ve increased architectural flexibility

When choosing whether to expose functionality in a microservice or in a shared dependency, we now more often choose putting the functionality in a Ruby gem. if developers had to manually update all the applications for each version bump this would be really expensive. This allows for easier testing and development.

### 3. Upgrade cycles are easier

Because we now get notified when a new version of a dependency arrives, it’s much easier for a single developer to review the changelog, make any changes to the applications, and apply the upgrade. Previously, an upgrade might be spread out across time, and be done by multiple people.
