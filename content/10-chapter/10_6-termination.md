---
title: "Termination"
pre: "10.6 "
weight: 116
date: 2018-08-24T10:53:26-05:00
---

## What is termination?

In this section, we will consider the notion of *termination* -- whether a function will ever finish running.

## Partial correctness vs total correctness

Up to this point, we have proved *partial correctness* for functions -- IF the function's precondition holds, AND if it terminates, THEN we promise that its postcondition will hold. 

### Example of partial correctness

Consider the following version of our `mult` function, which uses repeated addition to multiply two numbers:

```text
// #Sireum #Logika
//@Logika: --background save
import org.sireum._

def mult(m: Z, n: Z): Z = {
    Contract(
        Ensures(Res[Z] == m * n)
    )

    var sum: Z = 0
    var count: Z = 0

    while (count != n) {
        Invariant(
            Modifies(sum, count),
            sum == m * count
        )

    sum = sum + m
    count = count + 1
  }

  return sum
}
```

This function will be verified in Logika's auto mode, but in fact it has a subtle flaw. If we were to pass in `-1` for our second parameter (`n`), then we would get stuck in an infinite loop. `count` would be initially 0, and we would increment `count` each time in the loop, but of course it NEVER equal -1.

This is an example of partial correctness -- if our function DOES finish (which it would for nonnegative values of `n`), then we have shown it will return the correct value. We can see that we will need to require that the `n` parameter be nonnegative .

### Total correctness definition

*Total correctness* goes a step further than partial correctness -- it says that IF the function's precondition holds, THEN we promise that it will terminate and that its postcondition will hold. 

In order to show total correctness for our `mult` function, we must show that it always terminates. 

## Process of proving termination

We will see later in this section that the idea of termination is a much more challenging topic than it might seem. There is no button in Logika that will check for termination, but we can often insert manual assertions which, if they are verified, will guarantee termination. We will show how to create such manual assertions for simple loops that execute a set number of times.

First, we need to come up with a way to measure (as an integer) how much work the loop has left to do. Using this measure of work, we want to show two things:

- Each loop iteration decreases the integer measure (i.e., the amount of work left to do is strictly decreasing)
- When our integer measure is 0 or less, then we are certain that we are done (i.e., the loop exits)

## Termination in mult

In our `mult` example, let's first try to establish an integer measure of work for the loop. We know that the loop is computing `m + m + ... + m`, for a total of `n` additions. When `count` is 0, we know we have `n` more additions to do (`n` more iterations of the loop). When `count` is 1, we know we have `n-1` more additions...and when `count` is `n`, we know that we have no more additions to do (and the loop ends). Our measure of work should be the number of additions left to do, which is:

```text
Measure of work: n - count
```

We can calculate this measure at the beginning of each iteration and again at the end of each iteration:

```text
while (count != n) {
    Invariant(
        Modifies(sum, count),
        sum == m * count
    )

    //get measure value at beginning of iteration
    val measureBegin: Z = n-count

    sum = sum + m
    count = count + 1

    //get measure value at end of iteration
    val measureEnd: Z = n-count
}
```

Next, we want to assert that `measureEnd < measureBegin` -- that the amount of work decreases with each iteration. We can also assert that `measureEnd > 0 || count == n` -- that either we have more work to do, or our loop condition is false (meaning that if we have no more work to do, then our loop condition must be false and thus terminate):

```text
def mult(m: Z, n: Z): Z = {
    Contract(
        Requires (n >= 0),          //needed for termination
        Ensures(Res[Z] == m * n)
    )

    var sum: Z = 0
    var count: Z = 0

    while (count != n) {
        Invariant(
            Modifies(sum, count),
            sum == m * count
        )

        //get measure value at beginning of iteration
        val measureBegin: Z = n-count

        sum = sum + m
        count = count + 1

        //get measure value at end of iteration
        val measureEnd: Z = n-count

        //we are making progress
        //the amount of work decreases with each iteration
        assert(measureEnd < measureBegin)

        //we either have more work, or the loop will terminate
        //(if there is no work work to do, then the loop condition must be false)
        assert(measureEnd > 0 || count == n)     //NOTE: will not hold!
    }

    return sum
}
```

If we try verifying this program in Logika, the second assert, `assert(measureEnd > 0 || count == n)` will not hold. To see why, let's suppose that `measureEnd <= 0`. For the assert to be true, we would need to be certain that `count == n` (since the left side of the OR would be false). Because `measureEnd = n-count`, we can infer that `count >= n` when `measureEnd <= 0`. However, Logika is unable to infer that `count == n` from the knowledge that `count >= n` unless it also knows that `count <= n` always holds. We can add this knowledge by strengthening our loop invariant to provide a range for the loop counter -- `count >= 0` and `count <= n`. Even if not required, it is a good habit anyway to include the loop counter range as part of the loop invariant.

We strengthen our loop invariant, and both asserts will hold -- thus demonstrating termination:

```text
def mult(m: Z, n: Z): Z = {
    Contract(
        Requires (n >= 0),          //needed for termination
        Ensures(Res[Z] == m * n)
    )

    var sum: Z = 0
    var count: Z = 0

    while (count != n) {
        Invariant(
            Modifies(sum, count),
            sum == m * count,
            0 <= count,
            count <= n
        )

        //get measure value at beginning of iteration
        val measureBegin: Z = n-count

        sum = sum + m
        count = count + 1

        //get measure value at end of iteration
        val measureEnd: Z = n-count

        //we are making progress
        //the amount of work decreases with each iteration
        assert(measureEnd < measureBegin)

        //we either have more work, or the loop will terminate
        //(if there is no work work to do, then the loop condition must be false)
        assert(measureEnd > 0 || count == n)     //This holds now!
    }

    return sum
}
```

We could similarly use measures of work and manual assert statements to prove termination in some recursive functions. Here, we would demonstrate that a parameter value decreased with each recursive call, and that we either had more work to do or had reached the base case of our recursion (with no more recursive calls needed).

## Collatz function

While it is possible to prove termination for certain kinds of programs -- those that loop or make recursive calls a set number of times -- it is not possible to prove termination for all programs.

Consider the `collatz` function below:

```text
// #Sireum #Logika
//@Logika: --background save
import org.sireum._

def collatz(m: Z): Z = {
    Contract(
        Requires( m > 0 ),
        Ensures( Res[Z] == 1 )
    )

    var n: Z = m
    while (n > 1) {
        Invariant(
            Modifies( n ),
            n >= 1
        )

        if (n % 2 == 0) {
            n = n / 2
        } else {
            n = 3 * n + 1
        }
    }

    return n
}
```

We see that we must pass `collatz` a positve parameter, and that it promises to return 1 (no matter what the parameter is). It contains a loop that repeatedly modifies a current value (which is initially the parameter value):

- If the current number is even, we divide the number by 2
- If the current number is odd, we triple the number and add 1 

Suppose we compute `collatz(17)`. We can track the value of `n` as follows: 17, 52, 26, 13, 40, 20, 10, 5, 16, 8, 4, 2, 1. We can see that `n` does eventually reach 1, and that the program terminates in that case. We can similarly try other parameters, and will again see that we always end up with 1 (sometimes after a surprising number of iterations). But in fact:

- No one has proved that the Collatz function terminates for all positive numbers; and
- No one has found a positive number on which the Collatz function does not terminate

You may notice that part of the problem is that due to the nature of the problem, we cannot write a sufficient loop invariant describing what progress we have made towards our goal -- our only loop invariant is nearly identical to our loop condition. In cases where we *can* write a loop invariant that adequately describes our progress, we often have enough information to prove termination as well.

## Decidability and the Halting problem

It is an obvious question whether we could write a program to check whether another program always terminates. Unfortunately, this (the *Halting problem*) turns out to be impossible, as was demonstrated by Alan Turing. The Halting problem is an example of an *undecidable* problem in computer science -- a decision problem (a problem with a yes/no answer) that we can't correctly answer one way or another on all inputs, even if we have unlimited resources.