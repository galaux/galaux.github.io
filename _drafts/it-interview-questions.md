---
layout: post
title: IT Job interview questions
description: "Questions I was asked during IT jobs interviews"
category: articles
tags: [java]
---

## HTTP

- Cite all [HTTP method](https://tools.ietf.org/html/rfc7231#section-4.3) you know
  - [GET](https://tools.ietf.org/html/rfc7231#section-4.3.1), [HEAD](https://tools.ietf.org/html/rfc7231#section-4.3.2), [POST](https://tools.ietf.org/html/rfc7231#section-4.3.3), [PUT](https://tools.ietf.org/html/rfc7231#section-4.3.4), [DELETE](https://tools.ietf.org/html/rfc7231#section-4.3.5), [CONNECT](https://tools.ietf.org/html/rfc7231#section-4.3.6), [OPTIONS](https://tools.ietf.org/html/rfc7231#section-4.3.7), [TRACE](https://tools.ietf.org/html/rfc7231#section-4.3.8)
- What is the difference between POST and PUT?
  - The target resource in a POST request is intended to handle the enclosed representation according to the resource's own semantics, whereas the enclosed representation in a PUT request is defined as replacing the state of the target resource
- Explain what happens when an HTTP GET query is issued?
- What is a [_safe_ HTTP method](https://tools.ietf.org/html/rfc7231#section-4.2.1)?
  - Request methods are considered _safe_ if their defined semantics are essentially read-only; i.e., the client does not request, and does not expect, any state change on the origin server as a result of applying a safe method to a target resource. Likewise, reasonable use of a safe method is not expected to cause any harm, loss of property, or unusual burden on the origin server.
- Which HTTP methods are _safe_?
  - GET, HEAD, OPTIONS, and TRACE
- What does [HTTP idempotent method](https://tools.ietf.org/html/rfc7231#section-4.2.2) mean?
  - A request method is considered _idempotent_ if the intended effect on the server of multiple identical requests with that method is the same as the effect for a single such request.
- Which HTTP methods are idempotents?
  - PUT, DELETE, and safe request methods
- Thus, what is the difference in how GET and POST can be treated by a web proxy?
  - GET (and HEAD) can be cached by proxies. POST (and others) cannot.
- What does REST mean and how does work?
  - REpresentational State Transfer
  - TODO
- What does it mean for a web application to be stateless?
- If an application is stateless, how can we handle sessions?
  - Client side must send the session for each query

## Database

- What does [ACID](http://en.wikipedia.org/wiki/ACID) mean?
  - __Atomicity__: requires that each transaction be "all or nothing": if one part of the transaction fails, the entire transaction fails, and the database state is left unchanged
  - __Consistency__: ensures that any transaction will bring the database from one valid state to another. Any data written to the database must be valid according to all defined rules
  - __Isolation__: ensures that the concurrent execution of transactions results in a system state that would be obtained if transactions were executed serially
  - __Durability__: once a transaction has been committed, it will remain so, even in the event of power loss, crashes, or errors
- What does the [CAP](http://en.wikipedia.org/wiki/CAP_theorem) mean?
  - It is impossible for a distributed computer system to simultaneously provide all three of the following guarantees:
    - __Consistency__: all nodes see the same data at the same time
    - __Availability__: a guarantee that every request receives a response about whether it was successful or failed
    - __Partition tolerance__: the system continues to operate despite arbitrary message loss or failure of part of the system

## Programming

- What does [_volatile_](http://en.wikipedia.org/wiki/Volatile_variable#In_Java) mean in Java?
  - Implies a global ordering on the reads and writes to a volatile variable. Thus every thread accessing a volatile field will read its current value before continuing, instead of (potentially) using a cached value
- What does [_static_](http://en.wikipedia.org/wiki/Volatile_variable#In_Java) mean in Java?
  - Can be applied to a field, a method or an inner class. A static field, method or class has a single instance for the whole class that defines it
- What are the issues multi-threading programs can face?
  - race condition
  - deadlock
- List all object languages you know
  - C++, Java, Javascript, Perl, Python, Ruby and PHP
- List all functional languages you know
- What are the advantages of a functional language over a non-functional language?
- Are there things that can only be done with a functional language?
  - No, functional languages just bring some flexibility in some fields
- What are the differences between Scala and Haskell?
  - Haskell: no mutable, no object, native typeclasses
- What issue can recursive function raise?
  - Exceed the execution stack limit (StackOverflowError)
- What is the limit of a StackOverflowError on a JVM?
  - It depends on the amount of virtual memory allocated to the stack (which is tuned with -Xss)
- How do you avoid StackOverflowError?
  - Make your function tail-recursive
  - Use a trampoline
  - Use a free monad

## General Architecture

- What does AJAX mean and how does it work?
- What does CSRF mean and how does it work?
- Explain MVC
- What is a web resource?

- Write the code of the "fizz buzz" game (For all numbers from 1 to 100, if it is a multiple of 3, say "fizz", if it is a multiple of 5, say "buzz". If it both, say "fizzbuzz")
- How is "List" implemented in Scala?
  - A singly linked list
- Rewrite class "List" in pseudo-code
