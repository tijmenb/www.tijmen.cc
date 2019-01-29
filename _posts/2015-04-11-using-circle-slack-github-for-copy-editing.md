---
layout: post
title:  "Using Circle, Slack & GitHub for copy editing"
---

Since we started development on [GoBoat](https://www.goboat.nl/) we've relied on Slack for internal communication, GitHub for hosting the source code and CircleCI for continuous integration.

One of the real advantages of this turned out to be copy editing by non-technical members of the team.

What we wanted:

- Allow everyone on the team to write text for the website, so we can move fast
- Have the copy under version control, so we know what changed
- Guard against missing translations and missing arguments
- No unnecessary developer involvement

The following describes how we used [Rails' i18n functionality](http://guides.rubyonrails.org/i18n.html) to create a flexible workflow.

### 1. Make sure every line of text is in the locale files

We were lucky to have started off by using i18n because we contemplated adding English translations fairly early on. While that wasn't immediately necessary, it turned out to be essential in collaborating on the site copy.

One very important thing to think of is to use understandable keys for the translations. By default Rails allows you to use a shorthand in views:

```erb
# views/reservations/show.html
<%= t '.title' %>
```

Which you'd specify as such:

```yaml
# en.yml
reservations:
  show:
    title: 'Your reservation'
```

Which is fine for a Rails developer, but to someone who's never worked with Rails, a "show page" doesn't make much sense. We decided to go for a more verbose version that explained the context more fully:

```erb
# views/reservations/show.html
<%= t 'reservation_page.title' %>
```

Which you'd specify as such:

```yaml
# en.yml
reservation_page:
  title: 'Your reservation'
```

This also allowed us to refactor things and move code around, without changing the structure of the YAML file.

### 2. Be defensive about the copy

Turn on errors when a translation is missing. This will make sure tests fail fast when somebody accidentally removes a line:

```ruby
# config/application.rb
config.action_view.raise_on_missing_translations = true
```

It's also great to write specific tests for the YAML files. The following is a test we have to guarantee that all states of a reservation have a proper name specified:

```ruby
ReservationState.state_names.each do |name|
  it "has a translation for #{name}" do
    t("reservation_status.#{name}.full_name").should_not match 'missing'
  end
end
```

### 3. Allow team mates to edit locale files & commit

GitHub has fully fledged editing capabilities. You can edit a file, create a branch and pull request all from the UI.

![Copy editing](/media/copy-editing-github.png)

### 4. Let CircleCI run your tests

GoBoat has a comprehensive test suite, including integration tests for most pages. Our tests run on [CircleCI](https://circleci.com) whenever we push to GitHub.

To give instant feedback, we turned on the CirlceCI Slack integration, which pushes a message every time a build fails or succeeds.

![A CircleCI run](/media/copy-editing-circle.png)

### 5. CircleCI deploys

CircleCI also does our releases, so team members do not have to wait for a developer to deploy new copy:

![A Slack message](/media/copy-editing-slack.png)
