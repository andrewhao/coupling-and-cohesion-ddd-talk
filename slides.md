class: middle

# Highly Cohesive,<br />Loosely Coupled<br />\(& Very Awesome\)

#### A domain-driven approach to beautiful systems

---

class: middle

## Let's imagine tonight

We work at a hot new startup: Delorean. The Uber for time-travel!

# 💁⏳🚖🙋

---

class: middle

## OMG it's making so much money!

🔥 We're releasing features right and left! 💵

---

class: middle

## But the code is a mess!

As systems grow, they naturally want to decompose...

---

class: middle

## Proliferation of code

---

class: middle

```ruby
class RidesController
  def create
    passenger = Passenger.find(params[:passenger_id])
    driver = Driver.where(
      available: true,
      longitude: params[:longitude],
      latitude: params[:latitude]
    ).first
    driver.send_to!(passenger)
  end
end
```

---

class: middle

```ruby
class RidesController
  def create
    passenger = Passenger.find(params[:passenger_id])
    driver = Driver.where(
      available: true,
      longitude: params[:longitude],
      latitude: params[:latitude]
    ).first
    passenger.charge_credit_card!
    driver.send_to!(passenger)
  end
end
```

---

class: middle

```ruby
class RidesController
  def create
    passenger = Passenger.find(params[:passenger_id])
    driver = Driver.where(
      available: true,
      longitude: params[:longitude],
      latitude: params[:latitude]
    ).first
    passenger.charge_credit_card!
    Analytics.log_ride_created!
    driver.send_to!(passenger)
  end
end
```

---

class: middle

```ruby
class RidesController
  def create
    passenger = Passenger.find(params[:passenger_id])
    is_food = params[:ride_type] == 'food'
    driver = Driver.where(
      can_food_delivery: is_food,
      # ...
    ).first
    restaurant = Restaurant.find_by(meal_type: params[:meal_type])
    passenger.charge_credit_card!
    Analytics.log_ride_created!
    driver.itinerary.add(restaurant)
    driver.itinerary.add(passenger)
  end
end
```

---

## Code clutter in Rails

As the monolith grows, feature code is scattered across the app.

```
app/
  controllers/rides_controller.rb
  models/ride.rb
  helpers/ride_helper.rb
  services/do_a_ride_thing.rb
```

---

class: middle

## So here we are...

* Features take forever to release
* Regressions are high
* The code is really hard to untangle.

---

## The evolution of a feature

#### Feature: As a user, I want to hail a Delorean

* ...and I want to charge a credit card

--
* ...and the system should log to Google Analytics

--
* ...and I want to also do food delivery

--
* ...and I want to use our internal Driver Routing system

--
* ...and the Driver should receive a notification

--
* ...and sometimes we deliver puppies

---

class: middle center

# 😭

---

class: middle center

## Hi, I'm Andrew

Friendly neighborhood programmer at Carbon Five

---

class: middle center

![Carbon Five](http://www.carbonfive.com/images/c5-logo-vertical.png)

---

class: middle center

### Beautiful systems
---

class: middle center

### Beautiful systems are *highly cohesive*

---

class: middle center

### Beautiful systems are *loosely coupled*

---

class: middle

## Highly cohesive

Modules and their related entities are all organized to live logically
near each other, are easily accessible.

---

class: middle

## Loosely coupled

Modules do not entangle themselves with other concerns in the world.
Possible to evolve one area of the system independently or easily over
the other.

---

class: middle centered background-image-contain

![Cohesion and coupling](images/cohesion-coupling-diagram.gif)

---

class: middle

## Introducing Domain-Driven Design!

DDD is a set of principles, design tools, and code patterns.

---

class: middle

### It is *not* a (coding) framework

--
### It is *not* technology-specific

--
### It is *not* prescriptive

---

## Our goals today

Introduce Just Enough concepts from DDD to give us the right insights to draw boundaries in our code!

---

class: middle center

## __Strategic Design__

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
