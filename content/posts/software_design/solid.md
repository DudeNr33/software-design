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

## Single Responsibility Principle

### Summary

*A module should be responsible to one, and only one, actor.* [1]

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

### Problems of violating the principle

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

1. Clean Architecture: A Craftsman's Guide to Software Structure and Design  
ISBN-13: 978-0-13-449416-6
