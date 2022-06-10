---
title: "Summary"
pre: "9.5. "
weight: 104
date: 2018-08-24T10:53:26-05:00
---

Chapter 9 showed us how to write function contracts to specify the requirements and behavior of functions, and loop invariants to reason about the behavior and progress of loops. To wrap up, we briefly summarize the process for verifying a program that includes one or more functions with loops:

## Step 1: Write function contracts

Write a function contract for any function that doesn't already have one. Function contracts go just inside the function defintion, and look like:

```text
l"""{
    requires (preconditions)
    ensures (postconditions)
}"""
```

(The *modifies* clause is omitted, as we did not use it in this chapter. We will use the *modifies* clause in chapter 10.) The *preconditions* list any requirements your function has about the range of its parameters, and the *postconditions* describe the impact of calling the function (in this chapter, the postcondition always describes how the return value relates to the parameters.) If you're not sure what to write as the postcondition, try walking through your function with different parameters to get a sense for the pattern of what the function is doing in relation to the parameters. If you were given a Logika proof function, you will likely need to use it in the postcondition (and loop invariant) to describe the behavior.

## Step 2: Write loop invariant blocks

Write a loop invariant block for any loop that doesn't already have one. Loop invariant blocks go just inside the loop (before any code) and look like:

```text
l"""{
    invariant (loop invariants)
    modifies (variable list)
}"""
```

The *invariant* clause lists all loop invariants, which should describe the progress the loop has made toward its goal (the loop invariant will often greatly reasonable the postcondition for the enclosing function). Loop invariants occasionally need to specify the range of different variables, especially if the invariant uses Logika facts (which may only be defined for particular values) or if you need more information about the final value of a variable when a loop terminates. I recommend making a table of variable values for several iterations of your loop to get a sense of the relationship between variables -- this relationship is what will become the loop invariant.

The *modifies* clause lists all variables that are modified in the loop body.

## Step 3: Prove invariant holds before loop begins

In each loop, prove your invariant holds before the loop begins. You may need to pull in the function's precondition as a premise in this step. You must prove EXACTLY the claim in all pieces of the loop invariant. If your loop invariant involves a Logika fact, you may need to pull in a piece of the fact definition to help prove the invariant.

## Step 4: Prove invariant still holds at end of iteration

In each loop, prove your invariant still holds at the end of each iteration. Start by pulling in each part of the loop invariant as a premise before the loop body begins. Use logic blocks to process each statement in the body of the loop. By the end of the loop, you must prove EXACTLY the claim in all pieces of the loop invariant. (Again, if your loop invariant involves a Logika fact, you'll want to pull in a piece of the fact definition to help with this step.)

## Step 5: Prove the postcondition

Use the combination of the loop invariant and the negation of the loop condition to prove the postcondition just before your function ends.

## Step 6: Prove the precondition before each function call

Before any function call, prove exactly the precondition(s) for the function (using whatever values you are passing as parameters).

## Step 7: Use postcondition after each function call

After returning from each function call, pull the function's postcondition into a logic block as a premise (using whatever values you passed as parameters). Use this information to help prove any asserts.