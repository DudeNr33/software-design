---
title: "SOLID"
date: 2022-08-14T20:21:05+02:00
draft: true
# bookComments: false
# bookSearchExclude: false
---

# The SOLID design principles

**SOLID** stands for:

* **S**ingle Responsibility Principle (SRP)
* **O**pen Closed Principle (OCP)
* **L**iskov Substitution Principle (LSP)
* **I**nterface Segregation Principle (ISP)
* **D**ependency Inversion Principle (DIP)

They are six of the most fundamental design principles on the module and class level.
In the following, _module_ can always be exchanged with _class_ as well.

We will demonstrate the different principles by applying them to an example.
As a starting point, imagine a web client class that handles web requests and keeps track of
the requested resources and responses. The tracked requests can be printed in a report.  
In a first draft, the implementation could look like this:

{{< plantuml id="Starting Point" >}}
class WebClient {
    tracked_requests: List
    request(method, url, headers, body) -> response
    track_request(method, url, response) -> None
    print_report(outputfile) -> None
}
{{< /plantuml >}}

## Single Responsibility Principle

### Summary

> *A module should be responsible to one, and only one, actor.* [^1]

The SRP is often misinterpreted as *a module or class should only do one thing*.  
But it is more about avoiding that a single module changes for different reasons at different times.

### Problems of violating the principle

If a module or class is responsible to more than one actor, it has more than one reason to change.
There is a high probability that a change motivated by one actor will only be tested and validated in
the context of this actor's use cases. Regressions or unforeseen side effects for the other actors
may only be discovered when it is too late.

### Example

In our example, the `WebClient` has three different responsibilities:
1. Handling of web requests
2. Data aggregation
3. Reporting of said data

All of these can change for different reasons:
* A user could ask for the implementation of a retry mechanism when handling web requests
* A user could ask for persistent storage of the aggregated data
* A user could ask for a change in the report format

It is therefore better to separate all of these concerns into different classes:

{{< plantuml id="Refactor towards SRP" >}}
class WebClient {
    tracker: RequestTracker
    reporter: TextReporter
    request(method, url, headers, body) -> response
    print_report(outputfile)
}

class RequestTracker {
    tracked_requests: List
    track_request(method, url, response)
}

class TextReporter {
    tracker: RequestTracker
    print_report(outputfile)
}

WebClient --> RequestTracker : use
WebClient --> TextReporter : use
TextReporter --> RequestTracker : use
{{< /plantuml >}}

We have chosen to let our central `WebClient` class act as a facade to the now decoupled `TextReporter`, so that users of the original implementation
don't have to change the way they use our `WebClient`.

## Open Closed Principle

### Summary

> * *A module will be said to be open if it is still available for extension. For example, it should be possible to add fields to the data structures it contains, or new elements to the set of functions it performs.*
> * *A module will be said to be closed if [it] is available for use by other modules. This assumes that the module has been given a well-defined, stable description (the interface in the sense of information hiding).* [^2]

Implementing new features to a system should not require changes of existing code. It should be possible to add new functionality
purely by writing new code. This can be achieved mainly in two ways:

1. **Inheritance:** by inheriting from a concrete implementation, the behaviour of the class can be modified or extended without modifying the parent class.
2. **Polymorphism (interfaces / abstractions):** by providing abstractions in the form of *interfaces* or *abstract classes*, multiple concrete implementations can exist.

The latter is the preferred way, as *implementation inheritance* is frowned upon in OOP.

The OCP aims to minimize coupling, which in turn minimizes the impact of change.  
It will never be possible to guard a module against all possible reasons to change, and over-engineering your software to satisfy the OCP for all theoretically imaginable future extensions
is not feasible.  
The goal is to identify points of predicted variation and create a stable interface around them.

### Problems of violating the principle

Modification of existing code means that dependent client modules might have to change as well, resulting in a large changeset for a simple new feature.

Additionally, changes in existing code always holds the danger of regression bugs.

### Example

Imagine that our request-tracking web client becomes more popular, and a simple textual output is not enough anymore.
A more user-friendly HTML report must be implemented. In the future other report formats might be requested.  
Adding other report formats to our current design would require a change to `WebClient`, as it holds a direct reference to the concrete
implementation of the reporter class. By creating an abstract base class `FileBasedReporter` that defines only the interface, we can rely on this
abstraction rather than the concrete implementations:

{{< plantuml id="Refactor towards OCP" >}}
class WebClient {
    tracker: RequestTracker
    reporter: TextReporter
    request(method, url, headers, body) -> response
    print_report(outputfile)
}

class RequestTracker {
    tracked_requests: List
    track_request(method, url, response)
}

abstract FileBasedReporter {
    tracker: RequestTracker
    print_report(outputfile)
}

class TextReporter {
}

class HTMLReporter {
}

WebClient -down-> RequestTracker : use
WebClient -right-> FileBasedReporter : use
FileBasedReporter -down-> RequestTracker : use
TextReporter -up-|> FileBasedReporter : implements
HTMLReporter -up-|> FileBasedReporter : implements
{{< /plantuml >}}

The concrete `FileBasedReporter` implementation could be provided by configuration, or by passing the implementation to the constructor of `WebClient`.  
Our `WebClient` is now closed against modifications due to changes in the reporting format, and the design is open for extensions.

A similar approach can be taken for the `RequestTracker`, for example if variants like `InMemoryRequestTracker` or `PersistentRequestTracker` become necessary.

## Liskov Substitution Principle

### Summary

### Problems of violating the principle

### Example

## Interface Segregation Principle

### Summary

### Problems of violating the principle

### Example

## Dependency Inversion Principle

### Summary

### Problems of violating the principle

### Example

## References

[^1]: Martin, Robert C. *Clean Architecture: A Craftsman's Guide to Software Structure and Design*. Prentice Hall. ISBN-13: 978-0-13-449416-6
[^2]: Mayer, Bertrand (1988). *Object-Oriented Software Construction.* Prentice Hall. ISBN 0-13-629049-3.