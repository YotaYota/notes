# Clean Architecture

When software design done right, the maintaing phase is identified by:

- Requires a fraction of the human resources.
- Changes are simple and rapid.
- Defects are few and far between.
- Effort is minimized.
- Functionality and flexibility are maximized.

## Definition

```emph
The goal of software architecture is to minimize the human resources required to
build and maintain the required system.
```

The _measure_ of design quality is the effort required to meet the needs of the customer.
If the effort is low, and stays low throughout the lifetime of the system, the
design is good. If that effort grows with each new release, the design is bad.

To build a system with a design and an architecture that minimize effort and
maximize productivity, you need to know which attributes of system architecture
lead to that end.

## Behaviour and Structure

Behaviour and structure are two values that needs to remain high.

A change must be easy to make.

The difficulty of a change should only be proprtional to the scope of the change,
and not the shape of the change.

If the difficulty of change _is_ proprtional to the shape of the change, then
the cost of development will increase over time.

It's a quality of the architecture that it might prefer a certain shape over
another, but this will make new features harder and harder to implement.
Therefore architecture should be shape agnostical.

## Paradigms

Paradigms tell you which programming structures to use, and when to use them.

- Structured programming
- Object-oriented programming
- Functional programming

These three paradigms impose dicipline in different ways

- Structured programming imposes dicipline on direct transfer of control.
- Object-oriented programming imposes dicipline on indirect transfer of control.
- Functional programming imposes dicipline upon assignment.

These diciplines are linked to architecture

- We use polymorphism as the mechanism to cross architectural boundaries.
- We use functional programming tp impose dicipline on the location of and
access to data.
- We use structured programming as the algorithmic foundation of our modules.

### Structured Programming

Recursively decompose a program into a set of small provable functions. We can
then use tests to try and prove those small provable functions incorrect. If
such tests fail to prove incorrectness, then we deem the functions to be correct
enough for our purposes.

### Object-oriented Programming

_Dependency inversion_: the code use using another, does not need to name it.

Since OO languages provide safe and convinient polymorphism means that any
source code dependency, no matter where it is, can be inverted (by using interfaces).

OO languages means that software architects have absolute control over the
direction of source code dependencies.

OO is the ability, through the use of polymorphism, to gain absolute control
over every source code dependency in the system. It allows the architect to
create a plugin architecture, in which modules that that contain high-level
policies are independent of modules that contain low-level details. The
low-level details are regulated to plugin modules that can be deployed and and
developed independently from the modules that contain high-level policies.

## Functional Programming

Variables in functional languages do not vary.

Immutability means no problems with race conditions, deadlock conditions or
concurrent update problems.

A common practice is to segregate the application into immutable and mutable
components. Mutable components needs to be safeguarded from concurrency by
transactions. It is wise to push as much processing as possible into the
immutable components, and use as little as possible in the mutable components.

