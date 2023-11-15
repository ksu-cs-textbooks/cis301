---
title: "Assignment Statements"
pre: "8.5. "
weight: 94
date: 2018-08-24T10:53:26-05:00
---

Assignment statements in a program come in two forms -- with and without mutations. Assignments without mutation are those that give a value to a variable without using the old value of that variable. Assignments with mutation are variable assignments that use the old value of a variable to calculate a value for the variable.

For example, an increment statement like `x = x + 1` MUTATES the value of `x` by updating its value to be one bigger than it was before. In order to make sense of such a statement, we need to know the previous value of `x`.

In contrast, a statement like `y = x + 1` assigns to `y` one more than the value in `x`. We do not need to know the previous value of `y`, as we are not using it in the assignment statement. (We do need to know the value of `x`).

## Assignments without mutation

We have already seen the steps necessary to process assignment statements that do not involve variable mutation. Recall that we can declare as a `premise` any assignment statement or claim from a previous logic block involving variables that have not since changed.

For example, suppose we want to verify the following program so the assert statement at the end will hold:

```text
import org.sireum.logika._

val x: Z = 4

val y: Z = x + 2

val z: Z = 10 - x

//the assert will not hold yet
assert(y == z & y == 6)
```

Since none of the statements involve variable mutation, we can do the verification in a single logic block:

```text
import org.sireum.logika._

val x: Z = 4    

val y: Z = x + 2

val z: Z = 10 - x

l"""{
    1. x == 4               premise     //assignment of unchanged variable
    2. y == x + 2           premise     //assignment of unchanged variable
    3. z == 10 - x          premise     //assignment of unchanged variable
    4. y == 4 + 2           subst1 1 2
    5. z == 10 - 4          subst1 1 3
    6. y == 6               algebra 4
    7. z == 6               algebra 5
    8. y == z               subst2 7 6
    9. y == z ∧ y == 6      ∧i 8 6
}"""

//now the assert will hold
assert(y == z & y == 6)
```

Note that we did need to do `∧i` so that the last claim was `y == z ∧ y == 6 `, even though we had previously established the claims `y == z` and `y == 6`. In order for an assert to hold (at least until we switch Logika modes in chapter 10), we need to have established EXACTLY the claim in the assert in a previous logic block. 

## Assignments with mutation

Assignments with mutation are trickier -- we need to know the old value of a variable in order to reason about its new value. For example, if we have the following program:

```text
import org.sireum.logika._

var x: Z = 4

x = x + 1

//this assert will not hold yet
assert(x == 5)
```

Then we might try to add the following logic blocks:

```text
import org.sireum.logika._

var x: Z = 4

l"""{
    1. x == 4               premise     //from previous variable assignment
}"""

x = x + 1

l"""{
    1. x == x + 1           premise     //NO! Need to distinguish between old x (right side) and new x (left side)
    2. x == 4               premise     //NO! x has changed since this claim

}"""

//this assert will not hold yet
assert(x == 5)
```

...but then we get stuck in the second logic block. There, `x` is supposed to refer to the CURRENT value of `x` (after being incremented), but both our attempted claims are untrue. The current value of `x` is not one more than itself (this makes no sense!), and we can tell from reading the code that `x` is now 5, not 4.

To help reason about changing variables, Logika has a special `name_old` value that refers to the OLD value of a variable called `name`, just before the latest update. In the example above, we can use `x_old` in the second logic block to refer to `x`'s value just before it was incremented. We can now change our premises and finish the verification as follows:

```text
import org.sireum.logika._

var x: Z = 4

l"""{
    1. x == 4               premise     //from previous variable assignment
}"""

x = x + 1

l"""{
    1. x == x_old + 1       premise     //Yes! x equals its old value plus 1
    2. x_old == 4           premise     //Yes! The old value of x was 4
    3. x == 4 + 1           subst1 2 1  
    4. x == 5               algebra 3   //Could have skipped line 3 and used "algebra 1 2" instead
}"""

//now the assert will hold
assert(x == 5)
```

By the end of the logic block following a variable mutation, we need to express everything we know about the variable's current value WITHOUT using the `_old` terminology, as its scope will end when the logic block ends. Moreover, we only ever have one `_old` value available in a logic block -- the variable that was most recently changed. This means we will need logic blocks after each variable mutation to process the changes to any related facts. 

## Variable swap example

Suppose we have the following Logika program:

```text
import org.sireum.logika._

var x: Z = readInt()
var y: Z = readInt()

val temp: Z = x
x = y
y = temp

//what do we want to assert we did?
```

We can see that this program gets two user input values, `x` and `y`, and then swaps their values. So if `x` was originally 4 and `y` was originally 6, then at the end of the program `x` would be 6 and `y` would be 4. 

We would like to be able to assert what we did -- that `x` now has the original value from `y`, and that `y` now has the original value from `x`. To do this, we might invent dummy constants called `xOrig` and `yOrig` that represent the original values of those variables. Then we can add our assert:

```text
import org.sireum.logika._

var x: Z = readInt()
var y: Z = readInt()

//the original values of both inputs
val xOrig: Z = x
val yOrig: Z = y

val temp: Z = x
x = y
y = temp

//x and y have swapped
//x has y's original value, and y has x's original value
assert(x == yOrig & y == xOrig)     //this assert will not yet hold
```

We can complete the verification by adding logic blocks after assignment statements, being careful to update all we know (without using the `_old` value) by the end of each block:

```text
import org.sireum.logika._

var x: Z = readInt()
var y: Z = readInt()

//the original values of both inputs
val xOrig: Z = x
val yOrig: Z = y

l"""{
    1. xOrig == x           premise
    2. yOrig == y           premise
}"""

//swap x and y
val temp: Z = x
x = y
l"""{
    1. x == y                   premise     //from the assignment statement
    2. temp == x_old            premise     //temp equaled the OLD value of x
    3. xOrig == x_old           premise     //xOrig equaled the OLD value of x
    4. yOrig == y               premise     //yOrig still equals y
    5. temp == xOrig            algebra 2 3
    6. x == yOrig               algebra 1 4
}"""
y = temp
l"""{
    1. y == temp                premise     //from the assignment statement
    2. temp == xOrig            premise     //from the previous logic block (temp and xOrig are unchanged since then)
    3. yOrig == y_old           premise     //yOrig equaled the OLD value of y
    4. y == xOrig               algebra 1 2
    5. x == yOrig               premise     //from the previous logic block (x and yOrig are unchanged since then)
    6. x == yOrig ^ y == xOrig  ^i 5 4
}"""

//x and y have swapped
//x has y's original value, and y has x's original value
assert(x == yOrig & y == xOrig)     //this assert will hold now
```

Notice that in each logic block, we express as much as we can about all variables/values in the program. In the first logic block, even though `xOrig` and `yOrig` were not used in the previous assignment statement, we still expressed how the current values our other variables compared to `xOrig` and `yOrig`. It helps to think about what you are trying to claim in the final assert -- since our assert involved `xOrig` and `yOrig`, we needed to relate the current values of our variables to those values as we progressed through the program.