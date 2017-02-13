# Highly Cohesive
# Loosely Coupled
# & Very Awesome.

A Domain-Driven approach to beautiful systems

---

## Let's imagine tonight:

We work at a hot new startup: Delorean. The Uber for time-travel!

---

## OMG it's making so much money!

We're releasing features right and left!

---

## But the code is a mess!

Feature pressure - let's release!
We took some shortcuts. Okay, we took a lot of shortcuts.

---

## Rails isn't helping, either!

As the monolith grows, feature code is scattered across the app.

Where did that file go?

---

## So here we are...

Features take forever to release

Regressions are high

The code is really hard to untangle.

---

## Let's peek at a controller

```ruby
class RidesController
  def create
    passenger = Passenger.find_by(params[:id])
    location = GeographicPoint.from(params[:longitude], params[:latitude])
    if (params[:request_type] == :food)
      food = Food.find(params[:food])
      driver = Driver.where(available: true, delivering_food: food,
within: 5.miles, location: location)
    end
    driver = Driver.where(available: true, within: 5.miles, location: location)
    driver.send_notification!
    Analytics.create(:ride_event)
    passenger.charge_credit_card!
  end
end
```

## Hi, I'm Andrew

Friendly neighborhood programmer at Carbon Five

---

![Carbon Five](http://www.carbonfive.com/images/c5-logo-vertical.png)

---

## Beautiful systems are:

...highly cohesive.

Modules and their related entities are all organized to live logically
near each other, are easily accessible.

---


## Beautiful systems are:

...loosely coupled.

Modules do not entangle themselves with other concerns in the world.
Possible to evolve one area of the system independently or easily over
the other.

---

## Introducing Domain-Driven Design!

DDD is a set of principles, design tools, and code patterns.

It's more like an umbrella of various thoughts

---

## It is *not*

- A framework
- Technology-specific
- Prescriptive

---

### Our goals today

Introduce Just Enough concepts from DDD to give us the right insights to draw boundaries in our code!

Also known as: __Strategic Design__

Let's just jump into it!

---

## Observation 1: the Real World is Messy

* Your customer calls it a `Trip`
* The trip routing code calls it an `Itinerary`
* Marketing calls it a `Ride`
* The checkout code calls it a `Rideshare`

What is the correct term to use?

---

## Solution 1: Ubiquitous Language

Answer: They (can) all be correct.

The business is allowed to use different terms:

A Trip may mean something different to Marketing, than it does to the
Trip Routing Subsystem. That's OK!

---

## Apply It! ⚡️ Maintain a Glossary

A Glossary is simply a list of terms and definitions that apply to each
of your business contexts.

So for example, your Marketing team might have a Glossary:

And your Trip Routing engineering team might have another Glossary:

---

## Apply It! ⚡️ Keep the language clean:

If you see discrepancies in the code, fix it!

Agree on the correct terms as a team.

---

## Prior Art & Helpful Resources

* http://gorodinski.com/blog/2013/04/29/sub-domains-and-bounded-contexts-in-domain-driven-design-ddd/
