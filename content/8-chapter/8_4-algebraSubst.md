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

    // --> add proof block to evaluate what has happened in the program

program statement

    // --> add proof block to evaluate what has happened in the program

program statement

    // --> add proof block to evaluate what has happened in the program

(...more program statements)

    // --> add assert statements to express what our program did
```

We see that if our program involves user input, then we must consider whether our program will only work correctly for certain input values. In that situation, we express our assumptions using `assume` statements.

After each program statement, we must add a proof block to evaluate what changed on that line of code. We will see more details on these proof blocks throughout the rest of this chapter. Recall that the syntax for those proof blocks looks like:

```text
Deduce(
    //@formatter:off
    1  (    claim       ) by Justification,
    ...
    //@formatter:on
)
```

Finally, we add one or more `assert` statements to express what our program did. These are usually placed at the end of a program, but sometimes we have assert statements throughout the program to describe the progress up to that point.

## Algebra justification

The `Algebra*` justification can be used for ANY algebraic manipulation on previous claims. When using this justification, include all relevant proof line numbers in whatever order (you might use as few as zero line numbers or as many as 3+ line numbers).

### Example

Consider this example (which eliminates the Logika mode notation and the necessary import statements):

```text
var x: Z = 6
var y: Z = x

//this assert will not hold yet
assert (y == 6)
```

Following our process from above, we add proof blocks after each program statement. In these proof blocks, we start by listing the previous program statement as a premise:

```text
var x: Z = 6

Deduce(
    //@formatter:off
    1  (    x == 6      )   by Premise,
    //@formatter:on
)

var y: Z = x

Deduce(
    //@formatter:off
    1  (    y == x      )   by Premise,

    //@formatter:on

    //need claim "y == 6" for our assert to hold
)

//this assert will not hold yet
assert (y == 6)
```

For our assert to hold, we must have EXACTLY that claim in a previous proof block -- so we know we want our second proof block to include the claim `y == 6`.

Here is the program with the second proof block completed -- the assert statement will now hold.

```text
var x: Z = 6

Deduce(
    //@formatter:off
    1  (    x == 6      )   by Premise,
    //@formatter:on
)

var y: Z = x

Deduce(
    //@formatter:off
    1 (     y == x      )   by Premise,
    2 (     x == 6      )   by Premise,         //established in a previous proof block, and x is unchanged since then
    3 (     y == 6      )   by Algebra*(1, 2)   //we know y is 6 using the claims from lines 1 and 2
    //@formatter:on
)

//this assert will now hold yet
assert (y == 6)
```

We could have also deleted the first proof block in this example. We would still be able to claim `x == 6` as a premise in the last proof block, as `x` had not changed since being given that value.

## Subst_< and Subst_>

We have two deduction rules that involve substitution -- `Subst_<` and `Subst_>`. Both of these rules are similar to the find/replace feature in text editors. They preserve truth by replacing one proposition with an equivalent one.

The `Algebra*` justification will work for most mathematical manipulation. However, it will not work for any claim involving `∧`, `∨`, `→`, `F`, `∀`, `∃` -- in those cases, we will be required to use substitution instead.

### Subst_< justification

Here is the syntax for the `Subst_<` rule. In the example below, line `m` must be an equivalence (something equals something else). Line `n` can be anything.

```text
Deduce(
    //@formatter:off

    ...
    m (     LHS_M == RHS_M  )   by SomeJustification,
    ...
    n (     LINE_N          )   by SomeJustification,
    ...
    p (     claim           )   by Subst_<(m, n),

    //@formatter:on
)
```

`(claim)` rewrites `LINE_N` by substituting all ocurrences of `RHS_M` with `LHS_M`. The `_<` part of the justification name indicates the direction of the find/replace. You can think of as `LHS_M < RHS_M` (showing that each `LHS_M` is becoming a `RHS_M`). Here is an example:

```text
Deduce(
    //@formatter:off

    1 (     x + 1 == y - 4                          )   by SomeJustification,
    2 (     x*(x + 1) == (x + 1) + y                )   by SomeJustification,
    3 (     (x + 1)*(x + 1) == (x + 1) + (x + 1)    )   by Subst_<(1, 2)

    //@formatter:on
)
```

We wrote line 3 by replacing each occurence of `x + 1` with `y - 4`.

### Subst_> justification

Here is the syntax for the `Subst_>` rule. Just as with `Subst_<`, line `m` must be an equivalence (something equals something else). Line `n` can be anything.

```text
Deduce(
    //@formatter:off

    ...
    m (     LHS_M == RHS_M  )   by SomeJustification,
    ...
    n (     LINE_N          )   by SomeJustification,
    ...
    p (     claim           )   by Subst_>(m, n),

    //@formatter:on
)
```

Here, `(claim)` rewrites `LINE_N` by substituting all ocurrences of `LHS_M` with `LHS_M`. We can think of as indicating `LHS_M > RHS_M` (showing that each `RHS_M` is becoming a `LHS_M`). Here is an example:

```text
Deduce(
    //@formatter:off

    1 (     x + 1 == y                      )   by SomeJustification,
    2 (     x*y == (x + 1) + y              )   by SomeJustification,
    3 (     x*(x + 1) == (x + 1) + x + 1    )   by Subst_>(1, 2)

    //@formatter:on
)
```

We wrote line 3 by replacing each occurence of `y` with `x + 1`. Note that we put parentheses around our first replacement to ensure a product equivalent to the original statement.