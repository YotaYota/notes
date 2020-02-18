# Clean Architecture

## Chapter 1: Design and Architecture

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

- We use structured programming as the algorithmic foundation of our modules.
- We use polymorphism as the mechanism to cross architectural boundaries.
- We use functional programming to impose dicipline on the location of and
access to data.

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

### Functional Programming

Variables in functional languages do not vary.

Immutability means no problems with race conditions, deadlock conditions or
concurrent update problems.

#### Segregation of Mutability

A common practice is to segregate the application into immutable and mutable
components. Mutable components needs to be safeguarded from concurrency by
transactions. It is wise to push as much processing as possible into the
immutable components, and use as little as possible in the mutable components.

#### Event Sourcing

Store the transactions, not the state.

## Design Principles

### SOLID Principles

- SRP: The Single Responsibility Principle
- OCP: The Open-Closed Principle
- LSP: The Liskov Substitution Principle
- ISP: The Interface Segregation Principle
- DIP: The Dependency Inversion Principle

The goal of the principles is the creation of mid-level software structures that

- Tolerate change
- Are easy to understand
- Are the basis of components that can be used in many software systems

#### SRP: The Single Responsibility Principle

Each software module should have one, and only one, reason to change. Or more accurate

_A module should be responsible to one, and only one, actor._

Here, a _module_ can be thought of as a source file.

An _actor_ can be thought of as a common group of stakeholders.

The SRP comes at play, for example, when different modules uses the same
algorithm. SRP then states that the modules should _not_ be merged, because at a
later time one part using the algorithm might need a change but not the other.

[In this scenario, SRP actually promotes duplication of code.]

The SRP appears at three different levels

- Functions and classes
- Components (_Common Closure Principle_)
- Architectural (_Axis of Change_)

#### OCP: The Open-Closed Principle

For software systems to be easy to change, they must be designed to allow the
behaviour of those systems to be changed by adding new code, rather than
changing existing code.

If component A should be protected from changes in component B, then component B
should depend on component A.

The goal is to make the system easy to extend without incurring a high impact of
change. This goal is acomplished by partitioning the system into components,
and arranging those components into a dependency hierarchy that protects
higher-level components from changes in lower-level components.

#### LSP: The Liskov Substitution Principle

To build a system from interchangeable parts, those parts must adhere to to a
contract that allows those parts to be substituted one for another.

Barbara Liskov defined subtypes in terms of a substitution property:

```definition
If for each object o1 of type S, there is an object o2 of type T such that for
all programs P defined in terms of T, the behaviour of P is unchanged when o1 is
substituted for o2, then S is a subtype of T.
```

#### ISP: The Interface Segregation Principle

In general it is harmful to depend on things that contain more than you need. A
change in such a module would require changes in modules that should not be
affected by the change.

#### DIP: The Dependency Inversion Principle

Code that implements high-level policy should not depend on the code that
implements low-level details. Rather, details should depend on policies.

The most flexible systems are those in which source code refers only to
abstractions, not to concretions.

- Don't refer to volatile concrete classes. (Refer to abstract interfaces
instead, creation of objects should in general use _Abstract Factories_).
- Don't derive from volatile concrete classes.
- Don't override concrete functions.
- Never mention the name of anything concrete and volatile.

DIP violations cannot be entirely removed, but they can be gathered into a small
number of concrete components and kept separate from the rest of the system.

## Components

Components are the units of deployment. Well designed components always retain
the ability to be independently deployable, and therefore also independently developable.

