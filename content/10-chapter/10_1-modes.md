---
title: "Logika Modes"
pre: "10.1 "
weight: 110
date: 2018-08-24T10:53:26-05:00
---

Logika has different modes for programming logic verification. We can switch between modes by going to File->Settings->Tools->Sireum->Logika.

## Logika's "manual" mode

Up to now, we have been running Logika in "manual mode", where we list the following settings at the beginning of a file:

```text
// #Sireum #Logika
//@Logika: --manual --background save
import org.sireum._
import org.sireum.justification._
```

We are now reaching the point where additional practice in manual mode may no longer be a learning activity, and where the proof blocks after claim transformations can become dozens of lines long.

## Logika's auto mode

In Chapter 10, we will be switching to Logika's "auto mode", where we remove the `--manual` from our file settings. We also no longer need the `org.sireum.justification._` library:

```text
// #Sireum #Logika
//@Logika: --background save
import org.sireum._
```

Auto mode allows us to reason about our program by using ONLY invariants and function contracts. While the same work has to be done for a program to be verified (the precondition must be true before a function call, the loop invariant must be true before the loop begins, etc.), symexe mode does the work of analyzing your program statements to ensure that all parts of your loop invariant and function contract are satisfied. When you use symexe mode, you will only need to include a function contract for each function and a loop invariant block for each loop, and it will do the grunt work.

### Multiplication example

In section 9.3, we did a verification of a multiplication program using Logika's manual mode. Here is how we would write the verification of the same program using Logika's symexe mode:

```text
// #Sireum #Logika
//@Logika: --background save
import org.sireum._

def mult(m: Z, n: Z): Z = {
    Contract(
        Requires(m >= 0 & n >= 0),  //precondition: y should be nonnegative
        Ensures(Res[Z] == m * n)    //postcondition (we promise to return x*y)
    )

    var r: Z = 0
    var i: Z = 0

    while (i != n) {
        Invariant(
            Modifies(r, i),
            r == m * i
        )

        r = r + m
        i = i + 1
    }

    return r
}

//////////// Test code //////////////

var one: Z = 3
var two: Z = 4

var answer: Z = mult(one, two)
assert(answer == 12)
```

Note that the only proof blocks we needed to provide were the function contract and the loop invariant block.

### Pitfalls

When using this more advanced mode, it is not always obvious why Logika will not verify. Sometimes semantic errors in the program keep it from verifying; i.e. Logika has found a corner or edge case for which the program does not account. Other times the invariants and conditions do not actually help prove the goal in an assert or postcondition. Inevitably, sometimes it will be both.

In either case an option is to begin typing each proof-block as if in manual mode until you find the logical or programming error.