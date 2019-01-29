---
layout: post
title: Extract your state machine into a class
---

At [GoBoat](https://www.goboat.nl/) we use the [Workflow gem](https://github.com/geekq/workflow) for managing the state of the reservation. Instead of specifying the state machine directly in a model, we opted to extract the state machine into a separate object.

{% highlight ruby %}
class Reservation < ActiveRecord::Base
  ...
  def state
    @state ||= ReservationState.new(self)
  end
end
{% endhighlight %}

{% highlight ruby %}
class ReservationState
  include Workflow

  attr_reader :reservation

  def initialize(reservation)
    @reservation = reservation
  end

  workflow do
    state :requested do
      event :refuse, transitions_to: :refused
      event :accept, transitions_to: :accepted
    end
    ...
  end
end
{% endhighlight %}

### Advantages

Keeping the state machine outside of the model has a couple of advantages.

- Cleans up the model: large state machines will definitely cause [your class to go over 100 lines](https://robots.thoughtbot.com/sandi-metz-rules-for-developers), which is bad for readability and maintainability.
- Explicit: it's more explicit that methods call a state machine. Calling `reservation.state.accepted?` makes it instantly clear that we're dealing with a 'magic' Workflow-method here instead of a boolean attribute in the `reservations` table.
- Prevents collisions: state machines have the habit of tacking on many methods for each state and event (GoBoat's state machine adds about 60).
