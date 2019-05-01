---
layout: post
title: Structure and interpretation of computer programs (Abelson)
date: 2019-02-14
categories: [book]
tags: [computer science]
author: [Max Kossek]
description: Book Notes for the book Structure and interpretation of computer programs Harold Abelson
---


## 1 Building Abstraction with Procedures

### 1.1 The Elements of Programming
Languages have: primitive expressions (simplest entities), means of combination (compound elements built from expression) and means of abstraction (naming and manipulation of compound elements). `(+ (* 3 5) (- 10 6))` is an example of prefix notation used in scheme. Interpreter runs in a read-eval-print loop (REPL). A name is a *variable*; a value is *object* `(define size 2)`. Evaluation rule is recursive in nature. Program is in “environment”, a place in memory.
