---
title: "Programming Logic Goal"
pre: "8.1. "
weight: 90
date: 2018-08-24T10:53:26-05:00
---

In the next three chapters, we will learn how to reason about different kinds of program structures -- assignments, conditional statements, loops, function calls, recursion, lists of elements, and global variables. By the end of chapter 10, we will be able to prove the correctness of simple programs using a toy language that is a subset of Scala. While our toy language is not used in practice, the ideas that we will see to prove program correctness -- preconditions, postconditions, loop invariants, and global invariants -- can be used to specify functions in ANY language.

We will see that the process for formal specifications and proofs of correctness is rather tedious, even for relatively simple programs. And in practice, proving correctness of computer programs is rarely done. So why bother studying it?

## Safety critical code

One case where reasoning about correctness is certainly relevant is the arena of *safety critical code* -- where lives depend on a program working correctly. Certain medical devices, such as pacemakers and continuous glucose monitors, have a software component. If that software fails, then a person could die. We can't test the correctness of medical devices by installing them in a human body and trying them out -- instead, we need to be absolutely sure they work correctly before they are used.

Similarly, there is software in things like shuttle launches. While that might not cost lives, it's also a process that can't be fully tested beforehand. After all, no one is going to spend over a billion dollar on a "practice" launch. Instead, we need a way to more formally demonstrate that the software will work correctly.

## Specifications

In chapter 9, we will learn to write function *specifications*. These specifications list any requirements the function has in order to work correctly (*preconditions*) and descibe the impact of calling the function (*postconditions*) - most notably, what the function returns in terms of its inputs. Even in cases where we do not formally prove correctness of a program, it is very useful to write a specification for all functions. This can make it clear to any calling code what kinds of parameters should be passed, as well as what to expect about the returned value. By providing this structure, there will be fewer surprises and unintended errors.