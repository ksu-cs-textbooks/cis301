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

We have already seen the steps necessary to process assignment statements that do not involve variable mutation. Recall that we can declare as a `Premise` any assignment statement or claim from a previous proof block involving variables that have not since changed.

For example, suppose we want to verify the following program so the assert statement at the end will hold (this example again eliminates the Logika mode notation and the necessary import statements, which we will continue to do in subsequent examples):

```text
val x: Z = 4

val y: Z = x + 2

val z: Z = 10 - x

//the assert will not hold yet
assert(y == z & y == 6)
```

Since none of the statements involve variable mutation, we can do the verification in a single proof block:

```text
val x: Z = 4    

val y: Z = x + 2

val z: Z = 10 - x

Deduce(
    1 (     x == 4          )   by Premise,     //assignment of unchanged variable
    2 (     y == x + 2      )   by Premise,     //assignment of unchanged variable
    3 (     z == 10 - x     )   by Premise,     //assignment of unchanged variable
    4 (     y == 4 + 2      )   by Subst_<(1, 2),
    5 (     z == 10 - 4     )   by Subst_<(1, 3),
    6 (     y == 6          )   by Algebra*(4),
    7 (     z == 6          )   by Algebra*(5),
    8 (     y == z          )   by Subst_>(7, 6),
    9 (     y == z ∧ y == 6 )   by AndI(8, 6)
)

//now the assert will hold
assert(y == z & y == 6)
```

Note that we did need to do `AndI` so that the last claim was `y == z ∧ y == 6 `, even though we had previously established the claims `y == z` and `y == 6`. In order for an assert to hold (at least until we switch Logika modes in chapter 10), we need to have established EXACTLY the claim in the assert in a previous proof block. 

## Assignments with mutation

Assignments with mutation are trickier -- we need to know the old value of a variable in order to reason about its new value. For example, if we have the following program:

```text
var x: Z = 4

x = x + 1

//this assert will not hold yet
assert(x == 5)
```

Then we might try to add the following proof blocks:

```text
var x: Z = 4

Deduce(
    1 (     x == 4      )   by Premise  //from previous variable assignment
)

x = x + 1

Deduce(
    1 (     x == x + 1  )   by Premise, //NO! Need to distinguish between old x (right side) and new x (left side)
    2 (     x == 4      )   by Premise, //NO! x has changed since this claim
)

//this assert will not hold yet
assert(x == 5)
```

...but then we get stuck in the second proof block. There, `x` is supposed to refer to the CURRENT value of `x` (after being incremented), but both our attempted claims are untrue. The current value of `x` is not one more than itself (this makes no sense!), and we can tell from reading the code that `x` is now 5, not 4.

To help reason about changing variables, Logika has a special `Old(varName)` function that refers to the OLD value of a variable called `varName`, just before the latest update. In the example above, we can use `Old(x)` in the second proof block to refer to `x`'s value just before it was incremented. We can now change our premises and finish the verification as follows:

```text
var x: Z = 4

Deduce(
    1 (     x == 4      )   by Premise  //from previous variable assignment
)

x = x + 1

Deduce(
    1 (     x == Old(x) + 1     )   by Premise, //Yes! x equals its old value plus 1
    2 (     Old(x) == 4         )   by Premise, //Yes! The old value of x was 4
    3 (     x == 4 + 1          )   by Subst_<(2, 1),
    4 (     x == 5              )   by Algebra*(3)  //Could have skipped line 3 and used "Algebra*(1, 2)" instead
)

//now the assert will hold
assert(x == 5)
```

By the end of the proof block following a variable mutation, we need to express everything we know about the variable's current value WITHOUT using the `Old` terminology, as its scope will end when the proof block ends. Moreover, we only ever have one `Old` value available in a proof block -- the variable that was most recently changed. This means we will need proof blocks after each variable mutation to process the changes to any related facts. 

## Variable swap example

Suppose we have the following program:

```text
var x: Z = Z.read()
var y: Z = Z.read()

val temp: Z = x
x = y
y = temp

//what do we want to assert we did?
```

We can see that this program gets two user input values, `x` and `y`, and then swaps their values. So if `x` was originally 4 and `y` was originally 6, then at the end of the program `x` would be 6 and `y` would be 4. 

We would like to be able to assert what we did -- that `x` now has the original value from `y`, and that `y` now has the original value from `x`. To do this, we might invent dummy constants called `xOrig` and `yOrig` that represent the original values of those variables. Then we can add our assert:

```text
var x: Z = Z.read()
var y: Z = Z.read()

//the original values of both inputs
val xOrig: Z = x
val yOrig: Z = y

val temp: Z = x
x = y
y = temp

//x and y have swapped
//x has y's original value, and y has x's original value
assert(x == yOrig ∧ y == xOrig)     //this assert will not yet hold
```

We can complete the verification by adding proof blocks after assignment statements, being careful to update all we know (without using the `Old` value) by the end of each block:

```text
var x: Z = Z.read()
var y: Z = Z.read()

//the original values of both inputs
val xOrig: Z = x
val yOrig: Z = y

Deduce(
    1 (     xOrig == x  )   by Premise,
    2 (     yOrig == y  )   by Premise
)

//swap x and y
val temp: Z = x
x = y

Deduce(
    1 (     x == y              )   by Premise,     //from the assignment statement
    2 (     temp == Old(x)      )   by Premise,     //temp equaled the OLD value of x
    3 (     xOrig == Old(x)     )   by Premise,     //xOrig equaled the OLD value of x
    4 (     yOrig == y          )   by Premise,     //yOrig still equals y
    5 (     temp == xOrig       )   by Algebra*(2, 3),
    6 (     x == yOrig          )   by Algebra*(1, 4)
)

y = temp

Deduce(
    1 (     y == temp           )   by Premise,     //from the assignment statement
    2 (     temp == xOrig       )   by Premise,     //from the previous proof block (temp and xOrig are unchanged since)
    3 (     yOrig == Old(y)     )   by Premise,     //yOrig equaled the OLD value of y
    4 (     x == xOrig          )   by Algebra*(1, 2),
    5 (     x == yOrig          )   by Premise,     //from the previous proof block (x and yOrig are unchanged since)  
    6 (     x == yOrig ∧ y == xOrig )   by AndI(5, 4)
)

//x and y have swapped
//x has y's original value, and y has x's original value
assert(x == yOrig ∧ y == xOrig)     //this assert will hold now
```

Notice that in each proof block, we express as much as we can about all variables/values in the program. In the first proof block, even though `xOrig` and `yOrig` were not used in the previous assignment statement, we still expressed how the current values our other variables compared to `xOrig` and `yOrig`. It helps to think about what you are trying to claim in the final assert -- since our assert involved `xOrig` and `yOrig`, we needed to relate the current values of our variables to those values as we progressed through the program.