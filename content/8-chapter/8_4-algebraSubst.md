---
title: "Algebra and subst Rules"
pre: "8.4. "
weight: 93
date: 2018-08-24T10:53:26-05:00
---

In this section, we will learn our first two proof rules for programming logic -- `algebra` and `subst`.

## Verifying simple programs

Before we delve into our new proof rules, let's look at the process for verifying simple Logika programs (ones that include user input, variable initialization, and assignment statements using different operations). Here, the `// -->` lines are pieces of the verification process that you must add (or consider) in order to prove correctness of your program, and the other lines are the code of the program.

```text
get user input / set initial variable values

    // --> add assume statements to specify what must be true about the input

program statement

    // --> add logic block to evaluate what has happened in the program

program statement

    // --> add logic block to evaluate what has happened in the program

program statement

    // --> add logic block to evaluate what has happened in the program

(...more program statements)

    // --> add assert statements to express what our program did
```

We see that if our program involves user input, then we must consider whether our program will only work correctly for certain input values. In that situation, we express our assumptions using `assume` statements.

After each program statement, we must add a logic block to evaluate what changed on that line of code. We will see more details on these logic blocks throughout the rest of this chapter. Recall that the syntax for those logic blocks looks like:

```text
l"""{
    lineNumber. claim               justification

    // ... (more claims/justifications)
}"""
```

Finally, we add one or more `assert` statements to express what our program did. These are usually placed at the end of a program, but sometimes we have assert statements throughout the program to describe the progress up to that point.

## Algebra justification

The `algebra` justification can be used for ANY algebraic manipulation on previous claims. When using this justification, include all relevant proof line numbers in whatever order (you might use as few as zero line numbers or as many as 3+ line numbers).

### Example

Consider this example:

```text
import org.sireum.logika._

var x: Z = 6
var y: Z = x

//this assert will not hold yet
assert (y == 6)
```

Following our process from above, we add logic blocks after each program statement. In these logic blocks, we start by listing the previous program statement as a premise:

```text
import org.sireum.logika._

var x: Z = 6

l"""{
    1. x == 6               premise
}"""

var y: Z = x

l"""{
    1. y == x               premise

    //need claim "y == 6" for our assert to hold
}"""

//this assert will not hold yet
assert (y == 6)
```

For our assert to hold, we must have EXACTLY that claim in a previous logic block -- so we know we want our second logic block to include the claim `y == 6`.

Here is the program with the second logic block completed -- the assert statement will now hold.

```text
import org.sireum.logika._

var x: Z = 6

l"""{
    1. x == 6               premise
}"""

var y: Z = x

l"""{
    1. y == x               premise
    2. x == 6               premise     //established in a previous logic block, and x is unchanged since then
    3. y == 6               algebra 1 2 //we know y is 6 using the claims from lines 1 and 2
}"""

//this assert will hold
assert (y == 6)
```

We could have also deleted the first logic block in this example. We would still be able to claim `x == 6` as a premise in the last logic block, as `x` had not changed since being given that value.

## subst

We have two deduction rules that involve substitution -- `subst1` and `subst2`. Both of these rules are similar to the find/replace feature in text editors. They preserve truth by replacing one proposition with an equivalent one.

The `algebra` justification will work for most mathematical manipulation. However, it will not work for any claim involving `∧`, `∨`, `→`, `⊥`, `∀`, `∃` -- in those cases, we will be required to use substitution instead.

### subst1 justification

Here is the syntax for the `subst1` rule. In the example below, line `m` must be an equivalence (something equals something else). Line `n` can be anything.

```text
l"""{
    ...
    m. LHS_M == RHS_M       (some justification)
    ...
    n. LINE_N               (some justification)
    ...
    p. (claim)              subst1 m n
}"""
```

`(claim1)` rewrites `LINE_N` by substituting all ocurrences of `LHS_M` (the FIRST side of line `m`) with `RHS_M`. Here is an example:

```text
l"""{
    1. x + 1 == y - 4                       (some justification)
    2. x*(x + 1) == (x + 1) + y             (some justification)
    3. x*(y - 4) == (y - 4) + y             subst1 1 2
}"""
```

We wrote line 2 by replacing each occurence of `x + 1` with `y - 4`.

### subst2 justification

Here is the syntax for the `subst2` rule. Just as with `subst1`, line `m` must be an equivalence (something equals something else). Line `n` can be anything.

```text
l"""{
    ...
    m. LHS_M == RHS_M       (some justification)
    ...
    n. LINE_N               (some justification)
    ...
    p. (claim)              subst2 m n
}"""
```

`(claim1)` rewrites `LINE_N` by substituting all ocurrences of `RHS_M` (the SECOND side of line `m`) with `LHS_M`. Here is an example:

```text
l"""{
    1. x + 1 == y                               (some justification)
    2. y*(x + 1) == (x + 1) + y                 (some justification)
    3. (x + 1)*(x + 1) == (x + 1) + (x + 1)     subst2 1 2
}"""
```

We wrote line 2 by replacing each occurence of `y` with `x + 1`. Note that we put parentheses around our first replacement to ensure a product equivalent to the original statement.