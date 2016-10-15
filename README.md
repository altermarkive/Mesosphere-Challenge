# Mesosphere Challenge

Design and implement an elevator control system. What data structures, interfaces and algorithms will you need? Your elevator control system should be able to handle a few elevators — up to 16.

You can use the language of your choice to implement an elevator control system. In the end, your control system should provide an interface for:

* Querying the state of the elevators (what floor are they on and where they are going),
* receiving an update about the status of an elevator,
* receiving a pickup request,
* time-stepping the simulation.
* For example, we could imagine in Scala an interface like this:

```
    trait ElevatorControlSystem {
      def status(): Seq[(Int, Int, Int)]
      def update(Int, Int, Int)
      def pickup(Int, Int)
      def step()
    }
```

Here we have chosen to represent elevator state as 3 integers:

Elevator ID, Floor Number, Goal Floor Number

A pickup request is two integers:

Pickup Floor, Direction (negative for down, positive for up)

This is not a particularly nice interface, and leaves some questions open.
For example, the elevator state only has one goal floor; but it is conceivable
that an elevator holds more than one person, and each person wants to go
to a different floor, so there could be a few goal floors queued up.
Please feel free to improve upon this interface!

The most interesting part of this challenge is the scheduling problem.
The simplest implementation would be to serve requests in FCFS
(first-come, first-served) order. This is clearly bad; imagine riding
such an elevator! Please discuss how your algorithm improves on FCFS
in your write-up.

Please provide a source tarball (or link to a GitHub repository) containing
code in the language of your choice, as well as a README discussing
your solution (and providing build instructions). The accompanying documentation
is an important part of your submission. It counts to show your work.

Good luck!

# Missing information

* There is no mention of lift capacity so this needs to be added.
* There seems to be way of expressing the intention of the goal floor
  on behalf of a passenger (to be taken into account by the system once
  the passenger is picked up) - this needs to be added, for simplicity
  it will be added as a parameter of the pickup function.
* The purpose exposing the the update function is not explained enough
  but it is assumed here that the user of the inteface is not expected
  to be able to change the position and the direction of the lift arbitrarily.
  Thus, it is assumed here that the function is for internal use only.
* Next to the up/down directions (+/-) the state of the lift can also be idle
  (0).

# Improving the scheduling

Given that lifts have usually capacity bigger than 1 passenger first improvement
would be to allow the lift to opportunistically pick up "hitchhikers" along
the way in its direction (up to the capacity of the lift).

Once there are no more requests in need of serving in the direction the lift
is heading and it has no more passengers it will enter idle state. This
makes sure that the expectation of the passengers w.r.t. the direction is met.

The lifts will pick a direction only when they are in idle state
and the direction will be based on the position of the lift and the perceived
need for a pickup in that direction (to maximize the number passengers served).

# Assumptions

It will be assumed here that the passengers will only get on the lift only
if it goes in the direction of their intended floor.

It is also assumed that stops (contrary to a physical lift system) take no time
and the passengers enter and leave the lift momentarily.
