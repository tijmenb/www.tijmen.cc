---
title: 'Improving architectural introductions'
date: 2019/01/31
layout: post
---

Over the last year or so I've been doing "Intro to GOV.UK architecture" sessions every quarter. The audience is people who are new to GOV.UK and people who like a refresher.

We do this because GOV.UK publishing system is fairly complex — it consists of ~80 different components. Our [reference architecture diagram](https://docs.publishing.service.gov.uk/manual/architecture.html) looks like this at the moment:

![](/media/architecture-intros/1.png)

The format is simple: I draw GOV.UK's applications on a whiteboard, and I explain how content goes from a publishing app to the website, touching on things like email alerts, search, our Content Delivery Network (CDN).

I've iterated on the presentations a lot. These are some of the improvements I've made over time:

## 1. Ask for expectations

When people arrive, I ask them to write down their expectations on a post-it. This is a good starter because people will always trickle in in the first 5 minutes and this allows the people who join early to start thinking about the session (and the late people to join in easily, as they haven’t missed anything yet).

Once everybody has written down their hopes, I take the post-its, and read them out loud while putting them up on the wall. I make sure to call out anything that I definitely will cover, and the things that I won’t. At the end of the session I’ll check in on the post-its and ask if all the questions have been answered.

<figure>
<img src="/media/architecture-intros/2.jpeg" />
<figcaption>Picture of the end result in July 2018 — everything was handwritten and I tried to cover absolutely everything</figcaption>
</figure>

## 2. Shorter sessions

The sessions for developers used to run 90 minutes and sometimes they ran even longer.

While I still feel this is the correct amount of time, I’ve concluded that running anything over an hour is essentially useless because people won’t be able to recall much more afterwards, and risk getting super confused.

## 3. Neat drawing

Instead of hand writing all of the components on the wall, I now have a screenshot of GOV.UK to point to, and color-coded cards for each component in the system. This makes the board much easier to read, my handwriting is terrible, and allows me to re-layout the diagram if necessary.

<figure>
<img src="/media/architecture-intros/3.jpeg" />
<figcaption>Some improvements in August 2018 — I started experimenting with adding coloured notes</figcaption>
</figure>

## 4. Explain fewer things

I also cover a lot less things in the hour. I used to strive for completion, so let people see the entire ecosystem of GOV.UK, with all the edge cases, weird stuff and lesser components. Instead I now try to cover up edge cases, and focus more on the core applications.

## 5. Ask for feedback

Immediately after the session I send out a Google Form with 3 questions:

- What did you like about the session? (optional)
- What would it make it even better? (optional)
- Your name (optional)

The feedback has been invaluable in understanding which parts to focus on and what to improve.

<figure>
<img src="/media/architecture-intros/4.jpeg" />
<figcaption>The latest iteration in January 2019 — neat printouts for most of the components</figcaption>
</figure>
