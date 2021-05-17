Structured Programming - imposes discipline on direct transfer of control
do not use goto, use selection and repetition

Object–oriented Programming - imposes discipline on indirect transfer of control
Function became constructor, locals vars – instance vars, nested functions – methods.
Polymorphism through disciplined use of function pointers

Functional Programming - imposes discipline upon assignment

Dependency Inversion - when directions of the source code dependency and the flow of control can be different

OOP (architect pov) - ability through polymorphism to gain control over source code dependencies

Single Responsibility Principle (SRP) - A module should be responsible to one, and only one, actor.
At the level of components, it becomes the Common Closure Principle.
At the architectural level, it becomes the Axis of Change responsible for the creation of Architectural Boundaries

Open–Closed Principle (OCP) - A software artifact should be open for extension but closed for modification.
The goal is to make the system easy to extend without incurring a high impact of change. This goal is accomplished by partitioning the system into components, and arranging those components into a dependency hierarchy that protects higher–level components from changes in lower–level components.

Liskov Substitution Principle (LSP) - Objects of a superclass shall be replaceable with objects of its subclasses without breaking the application.

Can, and should, be extended to the level of architecture.

Interface Segregation Principle (ISP) - No client should be forced to depend on methods it does not use.
The lesson here is that depending on something that carries baggage that you don’t need can cause you troubles that you didn’t expect.

Dependency Inversion Principle (DIP) - The principle states:
– High–level modules should not depend on low–level modules. Both should depend on abstractions (e.g., interfaces).
– Abstractions should not depend on details. Details (concrete implementations) should depend on abstractions.

COMPONENT COHESION PRINCIPLES - REP: The Reuse/Release Equivalence Principle
CCP: The Common Closure Principle
CRP: The Common Reuse Principle

THE REUSE/RELEASE EQUIVALENCE PRINCIPLE - The unit of reuse is the unit of release. Effective reuse requires tracking of releases from a change control system. The package is the effective unit of reuse and release

THE COMMON CLOSURE PRINCIPLE - Gather into components those classes that change for the same reasons and at the same times. Separate into different components those classes that change at different times and for different reasons.

THE COMMON REUSE PRINCIPLE - Don’t force users of a component to depend on things they don’t need.
The CRP is the generic version of the ISP.

COMPONENT COUPLING PRINCIPLES - THE ACYCLIC DEPENDENCIES PRINCIPLE
THE STABLE DEPENDENCIES PRINCIPLE

THE ACYCLIC DEPENDENCIES PRINCIPLE - Allow no cycles in the component dependency graph

THE STABLE DEPENDENCIES PRINCIPLE - Depend in the direction of stability. Any volatile component should not be depend on a component that is difficult to change.

STABILITY METRICS - The SDP says that the I metric of a component should be larger than the I metrics of the components that it depends on.I (Instability) = Fan–out/(Fan–in + Fan–out). This metric has the range [0, 1]. I = 0 indicates a maximally stable component. I = 1 indicates a maximally unstable component.

THE STABLE ABSTRACTIONS PRINCIPLE - A component should be as abstract as it is stable.

MEASURING ABSTRACTION - The A metric is a measure of the abstractness of a component. Its value is simply the ratio of interfaces and abstract classes in a component to the total number of classes in the component.

The Humble Object pattern - Split the behaviors into two modules or classes. One contains all the hard–to–test behaviors
The other – all the testable behaviors

Good architectures are centered on - use cases so that architects can safely describe the structures that support those use cases without committing to frameworks, tools, and environments

Use cases describe - the application–specific rules that govern the interaction between the users and the Entities. How the data gets in and out of the system is irrelevant to the use cases

True and false duplications - True – every change to one instance necessitates the same change to every duplicate of that instance.
False – duplicates change at different rates, and for different reasons

