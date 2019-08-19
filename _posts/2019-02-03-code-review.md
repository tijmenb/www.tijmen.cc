---
title: 'How to code review a Rails application'
date: 2019/02/03
layout: post
---

Monday I spent the day at the offices of a government department. A few weeks ago I was asked to do a code review of a new application that they had built, specifically looking at security.

The application is built using Rails on Ruby - which currently isn't in use at the department. I've never written a report on a Rails app, so I'm still figuring out how to best approach the code review. What I've come up with so far:

### Authentication

How users sign in to the app, and which parts of the app can only be accessed by signing in. Look at the authentication library in use (a well known library like [devise](https://github.com/plataformatec/devise) is probably best)

### Authorisation

Whether there are different roles or permissions for users, and what actions they can perform.

### Dependencies

Which dependencies are used, are they up to date, and how trusted are they. Points for having Dependabot running.

### Rails configuration

Whether the application is configured with security in mind (mostly this would be following Rails' defaults).

### Advanced Rails security

Whether the application uses Rails' advanced security features like Content Security Policy (CSP) and protection against Cross-Site Request Forgery (CSRF).

### Cookies

Whether the session cookies are secure, and whether the application uses additional cookies.

### Exception reporting

If error reporting to something like Airbrake or Sentry is set up, and if it's configured to sanitise data before sending it.

### Linting

Whether the code is linted on CI. A linter Rubocop can catch bugs in development, and bugs can often lead to security vulnerabilities.

### Security tests

Whether there are any integration or unit tests relating to security, and their coverage.
