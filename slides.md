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

class: middle

## Our goals tonight

✏️ Learn insights to **draw boundaries** in our code!

--

🔍 See our system for what it truly is

---

class: middle center

### Also known as "Strategic Design"

---

class: middle

## Core domain

The **Core Domain** is the thing that your business does that makes it unique.

--

Delorean Core Domain: *Ridesharing*

---

class: middle

## Supporting domains

A **Supporting Domain** (or Subdomain) are the functions of the business
that play incidental roles in making the **Core Domain** happen.

--

_Delorean Supporting Domains:_

* **Driver Routing** (route me from X to Y)
* **Notifications** (push notifications)
* **Financial Transactions** (charge the card)
* **Product Analytics** (analysis of business metrics)

---

class: middle

## Apply It! ⚡️ Write down your domains

Sit down with stakeholders and map out your core and supporting domains.

---

## Language in the Real World

* Your customer calls it a **Trip**
* The trip routing code calls it an **Itinerary**
* Marketing calls it a **Ride**
* The checkout code calls it a **Rideshare**

What is the correct term to use?

---

## Answer: They (can) all be correct.

The business is allowed to use different terms:

A Trip may mean something different to Marketing, than it does to the
Trip Routing Subsystem. That's OK!

---

class: middle

## Ubiquitous Language

A set of linguistic terms your team uses to accurately describe business
functions, values and entities in the system **from the vantage point of
the domain**

---

## Apply It! ⚡️ Maintain a Glossary

A Glossary is simply a list of terms and definitions that apply to each
of your business contexts.

---

## Mo Glossaries, No Problems

So for example, your Marketing team might have a Glossary:

And your Trip Routing engineering team might have another Glossary:

---

## Apply It! ⚡️ Use consistent terms in code

If you see discrepancies in the code, fix it!

Agree on the correct terms as a team. Then use them universally
throughout the domain code.

---

## Bounded Contexts

A **Bounded Context** is:

- Concretely: a software system (like a codebase)
- Linguistically: a delineation in your domain where terms are
  "bounded", or have limited applicability.

--

Remember, this is because we agreed that different domains may have
different concepts, and hence different Ubiqutious Languages.

---

class: middle center

### I may have confused you.

---

#### Combining the concepts with a Context Map

### Step 1: Print out your UML diagram

---

#### Combining the concepts with a Context Map

### Step 2: Overlay your domains

---

#### Combining the concepts with a Context Map

### Step 3: Overlay your bounded contexts

---

#### Combining the concepts with a Context Map

### Step 4: Add directional dependencies

---

## Making sense of this

We may notice a few things:

--

* One bounded context contains multiple sub-(supporting) domains
* There are dependencies everywhere!

---

## Here's where we want to get to:

* Use a **Domain** as the ideal organizational module.
* Refactor entities that belong there to move into that module
* We will find that these entities naturally prefer each other, since
  they live in the same organizational unit.

---

## Extra insight

Conway's Law + Subdomains + Bounded Contexts are sometimes in a 1:1:1
ratio. The organization has optimized for communication within itself,
and has likely reduced its dependencies on other organizational units.

--

Thus this is a very natural place to draw a seam!

---

## Prior Art & Helpful Resources

* http://gorodinski.com/blog/2013/04/29/sub-domains-and-bounded-contexts-in-domain-driven-design-ddd/
