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

Consider a class that takes care of aggregating some data and printing a summary report of said data.
This class class violates the SRP, as data aggregation and report generation are two separate activities
with different reasons to initiate changes.

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