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
  ACID is a set of properties of database transactions where:
  - **Atomicity**: requires that each transaction be "all or nothing": if one part of the transaction fails, the entire transaction fails, and the database state is left unchanged
  - **Consistency** (watch-out: different from CAP's Consistency): ensures that any transaction will bring the database from one valid state to another. Any data written to the database must be valid according to all defined rules
  - **Isolation**: ensures that the concurrent execution of transactions results in a system state that would be obtained if transactions were executed serially
  - **Durability**: once a transaction has been committed, it will remain so, even in the event of power loss, crashes, or errors
- What is the [CAP theorem](http://en.wikipedia.org/wiki/CAP_theorem)?
  - It is impossible for a distributed shared-data system to simultaneously
    provide all three of the following guarantees:
    - **Consistency** (watch-out: different from ACID's Consistency): every read receives the most recent write or an error
    - **Availability**: every request receives a (non-error) response – without the guarantee that it contains the most recent write
    - **Partition tolerance**: the system continues to operate despite arbitrary message loss or failure of part of the system
- [What are SSTables and how they are used?](https://www.igvita.com/2012/02/06/sstable-and-log-structured-storage-leveldb/)
  - A Sorted String Table is a format used to persist huge amount of data on disk as a key→value hashmap
  - The SSTable is persited on disk and an index table of key→offset-of-value-on-disk is kept in memory
  - When performing a read, just go through the index table to get the hard drive offset of the value you are looking for
  - This ensure fast read
  - Once a SSTable is persisted, it is considered immutable, all later write go to a MemTable, a version of a SSTable but stored in memory
  - Then, when performing a read, just go through the MemTable first, if it does not contain a value for the key you are looking for, then go through the SSTable index table as you would normally
  - Once the MemTable is cluttered with to many updates, it is flushed to disk and becomes a new SSTable and we generate a new empty MemTable
  - We now have:
    - on disk:
      - 2 SSTables
    - in memory:
      - the 2 index tables of the 2 SSTables
      - an empty MemTable with its index table
  - When performing a read, first go through the MemTable, then sequentially through the SSTables' index tables
  - When performing a delete, just put a _tombstone_ for the corresponding key on the MemTable
  - Periodically perform compaction of the SSTables (and clear their index tables) to only retain the latest values


## Programming

- What are _concurrency_ and _parallelism_?
  - _concurrency_: capability to **manage** multiple tasks at the same time
  - _parallelism_: capability to **execute** multiple tasks at the same time
- What are the issues that programs using concurrent programming can face?
  - _race condition_ – ex: two tasks failing at incrementing the same mutable value
  - _deadlock_ – ex: two tasks trying to get a lock on two resources. Each task gets a lock on one resource and indefinitly waiting for the other resource to be freed.
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

### Java

- What does [_volatile_](http://en.wikipedia.org/wiki/Volatile_variable#In_Java) mean in Java?
  - Implies a global ordering on the reads and writes to a volatile variable. Thus every thread accessing a volatile field will read its current value before continuing, instead of (potentially) using a cached value
- What does [_static_](http://en.wikipedia.org/wiki/Volatile_variable#In_Java) mean in Java?
  - Can be applied to a field, a method or an inner class. A static field, method or class has a single instance for the whole class that defines it
- What is the difference between Java `Exception` and `Error` and should you `catch` them?
  - `Error`: a serious problems that a reasonable application should not try to catch
  - `Exception`: indicate conditions that a reasonable application might want to catch
  - All `Exception` but `RuntimeException`: need to be declared on the Jva method signature
  - [This graph as a picture](https://www.artima.com/javaseminars/modules/images/Checked.gif)
    ```
                              Throwable
                             /         \
                     Exception          Error
                    /  \   \  \          /|\
    RuntimeException    …   …   …       … … IOError
     / / / | \ \ \     /|\ /|\ /|\
    … … …  …  … … …   … … … … … … …
    ```

### Clojure

- What are Clojure's `future`, `delay` and `promise` (give use cases examples)?
  - Definitions
    - `future`:
      - define the body of a task
      - start the execution of this body immediatly
      - expect the result at some point in the future
    - `delay`:
      - define the body of a task
      - delay the execution of this body
      - (obviously) also expect the result later
    - `promise`:
      - there is no body but just a result expectation at some point in the future
  - Use cases
    - `future`:
      - concurrent I/O calls from which we **all** need the results
      - Run each call in a `future` then `deref` all of them to wait for **all** the results
      ```
      (doseq [f (repeatedly 5 #(future (big-job my-atom)))]
        (deref f))
      @my-atom
      ```
    - `delay`: concurrent I/O calls after which we want to run some code **only once**:
      - Define a _final_ task's body using `delay`
      - start concurrent I/O in `future`s which all then call `force` on the `delay`
      - The delay's body will only be called one (but its cached result will be returned each time)
    - `promise`: concurrent I/O calls from which we need **only the fastests** result:
      - Create a `promise` then each I/O in a `future` can call `deliver` but only the first call will return the value
- What are Clojure's vars? https://clojure.org/reference/vars
  - defined with `(def x …)`
  - by default are static (in a JVM sense, ie: all threads see the same value)
  - but can also be dynamic
    - `(def ^:dynamic *my-dyn* …)` (by convention, dynamic vars symbols are surrounded with `*`)
    - then, threads can use `(binding [*my-dyn* …])` to bind a new value **that this thread only will see**
  - `with-redefs` (and `with-redefs-fn`)
    - changes the root value of the var
    - all threads see the change
    - then the initial root value of the var is reinstated
  - alter-var-root
    - it takes a **symbol** as 1st argument
    - it takes a **function** as 2nd arguement
    - it applies its 2nd argument to the value of the symbol
      - thus why it is often used with `constantly` because we often don't care about the current value
    - it assigns the result to the root value of the var
    - all threads see the new value

- In Clojure, what are the differences between `dorun`, `doseq` and `doall`?
  - `dorun`
  |      | Holds whole | Chance to |         |
  |      | seq in mem? | do sthing | Returns |
  |      |             | on the seq|         |
  |------|-------------|-----------|-------- |
  |dorun |      ✗      |     ✗     |   nil   |
  |doseq |      ✗      |     ✓     |   nil   |
  |doall |      ✓      |     ✗     | the seq |
- What are the differences between symbols, vars and their bindings
  - var
    - is an instance of `clojure.lang.Var` which can be bound to a value
    - to return the instance of `clojure.lang.Var` for `the-value`
      - `(var the-value)`
      - `#'the-value`
  - a symbol
    - is an instance of `clojure.lang.Symbol` used as an identifier
    - to return the `clojure.lang.Symbol` of `the-value`
      - `'the-value`
      - `(symbol (var the-value))` (or `(symbol the-var)` if you have `the-var`)

#### `core.async`

- asynchronous communication between processes
- works on a pool of thread with default size 2+nb_of_cores
- `(go …)` runs the block of code inside a thread taken from the pool and
  immediatly returns a chan that will be passed the resuld of the last call of
  the block
- available functions:
  - put: `>!`
  - take: `<!`
- These are the **go block variants** of put and take:
  - _go block variants_ means:
    - they can only used inside a `(go …)` block
    - when invoked to put or take from a channel, if there is no value to put
      or take yet, the thread used for this will be _parked_: returned to the
      pool of threads
- **ordinary thread** variants: `>!!` and `<!!`
  - _ordinary thread variants_ means the thread used for this will **not** be
    returned to the pool of threads
  - `(thread …)` creates a new **ordinary thread** that can be used just like
    `(go …)` to run a block of code code in a different thread. `(thread …)` is
    very similar to a `(future …)` though the returned result is different.
  - it will return immediatly but contrary to `(go …)`, you can't use `>!` and
    `<!`, you must use `>!!` and `<!!` and this will ensure the underlying
    thread used to run block of code won't be returned to the pool of thread
    ([example use case: download files in
    parallel](https://www.braveclojure.com/core-async/#thread) where you don't
    want the block to be parked: you want the thread to still be assigned to
    the process so that the downloads go ahead)
- Important: both _go block variants_ and _ordinary thread variants_ block,
  waiting to put or take a value (watch-out for explanation in "Brave Clojure"
  where naming _Parking vs Blocking_ is a bit misleading).

- `alts!` and `alts!!`
  - take a vector of chans and return an array of the first element of the
    winning channel along with said winning channel
  - can take a timeout see
    [examples](https://www.braveclojure.com/core-async/#alts__)

#### Clojure Spec

Nota: as the first lines of the GitHub README explain:
- no need to add a dependency on `deps.edn`, Clojure already depends on it
- Clojure 1.9 depends on `spec.alpha`, Clojure 1.9+ depends on `core.specs.alpha`

The following apply to functions for which a corresponding `s/fdef` is declared:

- `s/check-asserts`
  - turns assertion checking `(s/assert …)` on/off
- `stest/instrument`
  - checks that arguments passed to the instrumented function satisfy `:args`
  - optionally accepts `:stub` and `:replace` parameters
  - don't forget to `stest/unstrument`
- `stest/check`
  - generates arguments based on `:args`, invoke the function, and checks that the `:ret` and `:fn` specs are satisfied

How to invoke generators: `(sgen/generate (s/gen …))`

### ClojureScript

- ClojureScript
  - is a transpiler (Input: Clojure code – Output: JS)
  - Uses Google Closure Compiler to generate minified/optimized JS code
  - Generated code can be a Node.js application, or plain JS one
- React
  - Facebook JS library of UI components and their lifecycle
- reagent
  - clojure wrapper around React to enable creation of React component in Clojure
  - emphasizes the use of form-1, form-2 and form-3 forms
  - simple to use to generate/use form-1 and form-2 forms
  - https://purelyfunctional.tv/guide/reagent/
- re-frame
  - wraps around `reagent` to ease the use of React Lifecycle Methods
  - ease the use of React's form-3 component and lifecycle
  - https://purelyfunctional.tv/guide/re-frame-lifecycle/


## General Architecture

- What does AJAX mean and how does it work?
- What does CSRF mean and how does it work?
- Explain MVC
- What is a web resource?

- Write the code of the "fizz buzz" game (For all numbers from 1 to 100, if it is a multiple of 3, say "fizz", if it is a multiple of 5, say "buzz". If it both, say "fizzbuzz")
- How is "List" implemented in Scala?
  - A singly linked list
- Rewrite class "List" in pseudo-code

- What is a statically/dynamically typed language?
  - defines when the typing will be checked: at compile time (_statically_) or at runtime (_dynamically_)
- _Strong_ and _weak_ are too fuzzy adjectives to describe a programming language
- we rather describe programming languages as being _type safe_ and/or _memory safe_
  - _type safety_ is the extent to which a programming language discourages/prevents **type** errors (_discourage_ and _prevent_ being up to anyone's point of view!)
  - _memory safety_ is the restriction on ability to read/write memory locations that the program is not supposed to access

                                ↑ dynamically typed
                                |
    JavaScript                  |                                 Clojure
    PHP                         |                                 Ruby
                                |
                                |
     ←––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––→
                                |                                     type safe
                                |
                                |
                                |                                 Scala, Go,
    C                           |                                 Java, Rust
                                ↓ statically typed




- `multimethods`: is **one** polymorphic operation based on **a dispatch function**
- `defprotocol`:
  - is **a collection** of polymorphic operations based on **the type of the first argument**
  - has better performances (because uses inner JVM primitives) TODO
  - example:
  ```
  (defprotocol MyProtocol
    (first-method [x] [x y]  ;; functions on protocols are called "methods"
    (second-method [x])))

  (extend-type java.lang.String
    MyProtocol
    (first-method [x] …)      ;; implementation
    (first-method [x] [y] …)  ;; implementation
    (second-method [x] …))    ;; implementation
  ```
  - One can use
    - `extend` or `extend-type` to declare the implementations of one or more protocoles by the same type
      - enable solving one side of the expression problem: _adding function(s) to a (collection of) structure(s)_
    - `extend-protocol` convenience function to provide implementations of the same protocol by different types all at once
      - enable solving the other side of the expression problem: _adding structure(s) supported by a (collection of) function(s)_

- `defrecord`:
  - map like data types
  - may or may not declare a protocol that it extends
