---
layout: post
title: IT Job interview questions
description: "Questions I was asked during IT jobs interviews"
category: articles
published: false
tags: [java]
---

## HTTP

- Cite all HTTP method you know
  - GET, HEAD, POST, PUT, DELETE, CONNECT, OPTIONS, TRACE
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
  - As GET is idempotent, it can be cached by proxies. POST cannot.
- What does REST mean and how does work?
  - __RE__presentational __S__tate __T__ransfer
  - TODO
- What does it mean for a web application to be stateless?
- If an application is stateless, how can we handle sessions?
  - Client side must send the session for each query

## Database

- What does [ACID](http://en.wikipedia.org/wiki/ACID) mean?
  - __A__tomicity: requires that each transaction be "all or nothing": if one part of the transaction fails, the entire transaction fails, and the database state is left unchanged
  - __C__onsistency: ensures that any transaction will bring the database from one valid state to another. Any data written to the database must be valid according to all defined rules
  - __I__solation: ensures that the concurrent execution of transactions results in a system state that would be obtained if transactions were executed serially
  - __D__urability: once a transaction has been committed, it will remain so, even in the event of power loss, crashes, or errors
- What does the [CAP](http://en.wikipedia.org/wiki/CAP_theorem) mean?
  - It is impossible for a distributed computer system to simultaneously provide all three of the following guarantees:
    - __C__onsistency: all nodes see the same data at the same time
    - __A__vailability: a guarantee that every request receives a response about whether it was successful or failed
    - __P__artition tolerance: the system continues to operate despite arbitrary message loss or failure of part of the system

## Programming

- What does [_volatile_](http://en.wikipedia.org/wiki/Volatile_variable#In_Java) mean in Java?
  - Implies a global ordering on the reads and writes to a volatile variable. Thus every thread accessing a volatile field will read its current value before continuing, instead of (potentially) using a cached value
- What does [_static_](http://en.wikipedia.org/wiki/Volatile_variable#In_Java) mean in Java?
  - Can be applied to a field, a method or an inner class. A static field, method or class has a single instance for the whole class that defines it
- What are the issues multi-threading programs can face?
  - race condition
  - deadlock

- Citez tous les langages objet que vous connaissez
  - C++, Java, Javascript, Perl, Python, Ruby and PHP
- Citez tous les langages fonctionnels que vous connaissez
- Quels sont les avantages d'un langage fonctionnel sur un langage non fonctionnel?
- Y-a-t'il des choses qu'on ne peut faire qu'avec un langage fonctionnel?
  - Non, les langages fonctionnels apportent juste une certaines flexibilité dans certains domaines
- Est-ce que JS est un langage fonctionnel?
- Quelle est la différence entre Scala et Haskell
  - Haskell: pas de mutable, pas d'objet, les typeclasses de base, …
- quel est le problème d'un appel récursif?
  - Possibilité de dépassement de pile d'exécution (__StackOverflowError__)
- quelle est la limite de profondeur du StackOverflowError sur une JVM?
  - It depends on the amount of virtual memory allocated to the stack (which is tuned with -Xss)
- comment éviter un StackOverflowError?
  - Coder la méthode pour qu'elle soit tail-recursive
  - si tu ne peux pas etre tail recursive => trampoline ou mieux : free monad

## General Architecture

- Expliquez-moi AJAX et comment ça fonctionne
- Pareil pour CSRF
- Qu'est-ce que MVC? Faites un dessin. Est-ce que je peux avoir un lien direct entre modèle et vue?
- qu'est ce qu'une resource HTTP

- Écrivez le code du jeu du "fizz buzz" (pour tous les nombres de 1 à 100, s'il est multiple de 3 dites "fizz" et s'il est multiple de 5 dites "fuzz" et s'il est les 2 dites "fizzbuzz")
- Comment est implémentée "List" en Scala?
- Écrivez-moi une classe "List" dans le (pseudo)langage que vous voulez
