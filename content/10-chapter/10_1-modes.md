---
title: "Logika Modes"
pre: "10.1 "
weight: 110
date: 2018-08-24T10:53:26-05:00
---

Logika has different modes for programming logic verification. We can switch between modes by going to File->Settings->Tools->Sireum->Logika.

## Logika's "manual" mode

Up to now, we have been running Logika in "manual mode", which uses these Logika settings:

![manual mode](/images/sireumManual.png)

We are now reaching the point where additional practice in manual mode may no longer be a learning activity, and where the proof-blocks after claim transformations can become dozens of lines long.

## Logika's SymExe mode

In Chapter 10, we will be switching to Logika's "symexe mode", which uses these Logika settings:

![symexe mode](/images/sireumSymexe.png)

Symexe mode allows us to reason about our program by using ONLY invariants and function contracts. While the same work has to be done for a program to be verified (the precondition must be true before a function call, the loop invariant must be true before the loop begins, etc.), symexe mode does the work of analyzing your program statements to ensure that all parts of your loop invariant and function contract are satisified. When you use symexe mode, you will only need to include a function contract for each function and a loop invariant block for each loop, and it will do the grunt work.

### Multiplication example

In section 9.3, we did a verification of a multiplication a program using Logika's manual mode. Here is how we would write the verification of the same program using Logika's symexe mode:

```text
import org.sireum.logika._

def mult(x: Z, y: Z) : Z = {
    //function contract
    l"""{
        requires y >= 0         //precondition: y should be nonnegative
        ensures result == x*y   //postcondition (we promise to return x*y)
    }"""

    var sum: Z = 0
    var count: Z = 0

    while (count != y) {
        l"""{
            invariant sum == count*x
            modifies sum, count
        }"""

        sum = sum + x
        count = count + 1
    }

    return sum
}

//////////// Test code //////////////

var one: Z = 3
var two: Z = 4

var answer: Z = mult(one, two)
assert(answer == 12)
```

Note that the only logic blocks we needed to provide were the function contract and the loop invariant block.

### Pitfalls

When using this more advanced mode, it is not always obvious why Logika will not verify. Sometimes semantic errors in the program keep it from verifying; i.e. Logika has found a corner or edge case for which the program does not account. Other times the invariants and conditions do not actually help prove the goal in an assert or postcondition. Inevitably, sometimes it will be both.

In either case an option is to uncheck "auto" and begin typing each proof-block as if in manual mode (this can be done with Symexe enabled) until you find the logical or programming error.