
---
layout: overview
title: A Scala Tutorial for Ruby Programmers
overview: scala-for-ruby-programmers

disqus: true
multilingual-overview: false
---

By Nathaniel Lim

## Introduction

This document gives a detailed introduction to features of the Scala
language using motivating aspects of the Ruby language and associated patterns.
It assumes at least an intermediate understanding of Ruby and demonstrates how
many of the comman design patterns and ideas from Ruby can be captured in a more
elegant, powerful, and safer manner using Scala.

## Hello World


## Scripting and the REPL

One of the great things about developing applications in Ruby is that you
can test out functions interactively in a REPL (Read-Eval-Print-Loop).  For instance
you heard of this great Ruby function called `map` you want to try it out.

    $ irb
    1.9.3p194 :001 >   ["ruby", "scala"].map { |s| s + " is fun!" }
      => ["ruby is fun!", "scala is fun!"]


## Type Inference

Scala is a statically-typed language, meaning that the type of every value, variable
and function (with their arguments) are known at compile time.  But did you notice that
there is very little boilerplate in terms of type annotations.
## Seq, Map syntax
## Everything is an Object
## Class versus Instance Methods, Companion object and Class
## Abstract classes definition,
## Case Switch, Regex
## Blocks, Procs, Lambdas, Methods become first class
## Lazy Enumeration, Lazy Evaluation - Stream
## Verified Mix-In Composition
## Monkey-Patching - Pimp my Library (PML) Pattern,
## Duck Typing - Structural Types, Context Bound, more safety

In Ruby, with its dynamic typing, it becomes feasible to design a function
that only requires that the argument to that function has a certain method.
For instance, say I wanted to print an objects imitation of a duck.  So I
require that the object I pass into the method has a quack method. In Ruby
this would look like:

      def printQuack(a)
        puts a.quack
      end

In Scala, as you have seen, we need to give the arguments to our function some type,
and then the compiler will infer the rest.  One solution here is to use structural-types.

      def printQuack(a: { def quack : String } ) = println(a.quack)

But you will find out if you look up the implementation of structural types on the JVM,
that it is very ineffiecient due to the use of reflection.

A more efficient solution that draws from the power of Scala object-orientation is to
define a trait for the quack method constraint.

     trait Quacker {
        def quack: String
     }

    def printQuack(a: Quacker) = println(a.quack)


Scalac can now check at compile-time, whether or not the argument `a` has the Quacker
trait mixed in and implemented.

Another Scala solution draws from the functional language pattern of typeclasses.


    trait QuackManual[T] {
      def quack(a: T): String
    }

    implicit object DuckQuackManual extends QuackManual[Duck] {
      def quack(a: Duck) = "quack!"
    }

    def printQuack[A](a: A)(implicit qm: QuackManual[A]) = println(qm.quack(a))




