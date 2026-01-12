---
emoji: 𝑓
title: Functional programming.
description: Looking at the state of the art web framework for elixir
date: 2025-10-25
layout: base
tags: ["tech", "programming"]
---

functional programming (FP) has always seemed like a curiosity – a realm of mathematical purity with little bearing on the practical realities of building robust, scalable software. However, as the complexity of software grows and the demand for concurrent and parallel processing. This post aims to demystify functional programming for the experienced object-oriented programmer and offer a technical bridge from the familiar landscapes of Rust, Python, and C++.

### No more Mutable State

Object-oriented programming (OOP) is good for modeling the world through a collection of interacting objects, each encapsulating its own state and behavior. This approach, while intuitive is a source of complexity: mutable state. The ability for an object's state to change over time can lead to a cascade of side effects, making it difficult to reason about the program's behavior, especially in concurrent environments.

Functional programming, in contrast, is a declarative paradigm that treats computation as the evaluation of mathematical functions and avoids changing-state and mutable data. This fundamental difference in philosophy gives rise to several core tenets that offer substantial advantages in modern software development.

### Immutability, Pure Functions, and Higher-Order Functions

#### Immutability == Predictability

Functional programming is based on **immutability**, the principle that data, once created, cannot be changed. In an OOP context, we are accustomed to modifying an object's attributes. In FP, any "modification" results in the creation of a new data structure, leaving the original untouched. This does look inefficient at first glance, but it provides a powerful guarantee: a piece of data, once created, is a stable, unchanging fact.

This immutability is really good for concurrency. In multi-threaded applications, the primary source of bugs like race conditions and deadlocks is the shared, mutable state. When data is immutable, it can be safely shared among multiple threads without the need for complex and error-prone locking mechanisms. This leads to simpler, more reliable concurrent code. Research has shown that functional approaches can be beneficial in constructing correct programs, especially when combined with formal specifications.

#### Reusability

A **pure function** is a function that adheres to two strict rules:

1.  **Its output is solely determined by its input.** It does not depend on any hidden state or external factors like I/O, global variables, or random number generators.
2.  **It has no side effects.** It does not modify any state outside of its own scope. This means no changing of global variables, no writing to a database, and no printing to the console.

This purity brings about a property known as **referential transparency**. An expression is referentially transparent if it can be replaced by its value without changing the program's behavior. For example, the expression `2 + 3` is referentially transparent because it can be replaced with `5` anywhere it appears. This property makes it significantly easier to reason about and debug code. You can analyze a pure function in complete isolation, confident that its behavior is consistent and predictable.

From a testing perspective, pure functions are a dream. With no external dependencies or side effects, writing unit tests becomes a straightforward exercise of providing inputs and asserting the corresponding outputs. This leads to more comprehensive and reliable test suites.

#### Higher-Order Functions

Functional programming treats functions as **first-class citizens**. This means that functions can be treated like any other data type: they can be passed as arguments to other functions, returned as values from other functions, and stored in variables. Functions that take other functions as arguments or return them as results are called **higher-order functions**.

Higher-order functions are a powerful tool for abstraction. They allow us to write generic, reusable code that can be customized with specific behaviors. For an OOP developer, this is analogous to the Strategy or Command design patterns, but often with much less boilerplate. Common examples of higher-order functions include `map`, `filter`, and `reduce`, which allow for expressive and concise manipulation of data collections.

### FP vs. OOP

It's crucial to understand that functional programming is not inherently "better" than object-oriented programming; they are different tools for different jobs. A comparative analysis of the two paradigms reveals their distinct strengths and weaknesses. While OOP excels at modeling complex entities with stateful behavior, FP shines in data processing and concurrent systems.

A study comparing implementations of a digital wallet system in Kotlin (OOP) and Scala (FP) highlighted these differences. The functional approach was often more concise and easier to reason about due to the absence of side effects. Another case study comparing an "Average Score" program implemented in both styles found that the functional-oriented design demonstrated superior readability and performance, while the object-oriented design excelled in maintainability and code reuse through inheritance and polymorphism.

### Functional Programming eg.

The good news for experienced programmers is that you don't need to switch to a purely functional language like Haskell to reap the benefits of FP. Many modern multi-paradigm languages, including Rust, Python, and C++, have excellent support for functional programming concepts.

#### FP in Rust

Rust, with its strong emphasis on safety and performance, incorporates many functional programming features. Its ownership and borrowing system, while not a purely functional concept, aligns well with the goal of managing state in a controlled manner. Rust's support for closures, iterators, and pattern matching makes it a powerful language for writing in a functional style.

**OOP in Rust:**
```rust
struct Point {
    x: i32,
    y: i32,
}

impl Point {
    fn new(x: i32, y: i32) -> Self {
        Point { x, y }
    }

    fn translate(&mut self, dx: i32, dy: i32) {
        self.x += dx;
        self.y += dy;
    }
}

let mut p = Point::new(1, 2);
p.translate(3, 4);
// p is now { x: 4, y: 6 }
```

**Functional in Rust:**

```rust
#[derive(Clone, Copy)]
struct Point {
    x: i32,
    y: i32,
}

fn translate(p: Point, dx: i32, dy: i32) -> Point {
    Point {
        x: p.x + dx,
        y: p.y + dy,
    }
}

let p1 = Point { x: 1, y: 2 };
let p2 = translate(p1, 3, 4);
// p1 is still { x: 1, y: 2 }, p2 is { x: 4, y: 6 }
```

#### Python: A Dynamic Approach to Functional Programming

Python has long supported functional programming constructs. Its dynamic nature and first-class functions make it easy to work with higher-order functions and lambdas. Libraries like `functools` and `itertools` provide a rich set of tools for functional-style programming.

**OOP in Python:**
```python
class Counter:
    def __init__(self):
        self.count = 0

    def increment(self):
        self.count += 1

counter = Counter()
counter.increment()
# counter.count is now 1
```

**Functional in Python:**
```python
def increment(count):
    return count + 1

count = 0
new_count = increment(count)
# count is still 0, new_count is 1
```

#### C++

Modern C++ has increasingly embraced functional programming idioms. The introduction of lambdas in C++11, along with the Standard Template Library (STL) algorithms like `std::transform` and `std::accumulate`, provides powerful tools for writing functional-style code. The `const` keyword can be used to enforce immutability to a certain degree.

**OOP in C++:**
```cpp
#include <iostream>

class Greeter {
private:
    std::string greeting;

public:
    Greeter(const std::string& g) : greeting(g) {}

    void greet(const std::string& name) {
        std::cout << greeting << ", " << name << "!" << std::endl;
    }
};

Greeter g("Hello");
g.greet("World");
```

**Functional in C++:**
```cpp
#include <iostream>
#include <string>
#include <functional>

auto make_greeter(const std::string& greeting) {
    return [greeting](const std::string& name) {
        std::cout << greeting << ", " << name << "!" << std::endl;
    };
}

auto greeter = make_greeter("Hello");
greeter("World");
```


