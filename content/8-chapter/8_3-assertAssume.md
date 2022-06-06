---
title: "Assert and Assume"
pre: "8.3. "
weight: 92
date: 2018-08-24T10:53:26-05:00
---

## Assert statements

An *assert* statement in Logika uses the syntax `assert(expression)`, where `expression` is of type `bool`. The assert passes if the expression is true, and throws an error if the expression is false.

Logika assert statements are different than assert statements in languages like Java and C# -- in those languages, the code in the assert statement (which often includes test method calls) is actually run, and the assert statement checks whether those methods are returning the expected values. In Logika, assert statements only pass if we have previously PROVED what we are claiming -- the code is never executed. 

### Example

Consider the following program:

```text
import org.sireum.logika._

val x: Z = 6
val y: Z = 6
val z: Z = 4

assert (x == y & y > z)
```

While we can tell that `x` equals `y` and that `y` is greater than `z`, this code would fail in Logika (at least in its manual mode, which is what we are using now). In order for an assert to pass, we must have already proved EXACTLY the statement in the assert.

Why do WE know that `x` equals `y` and that `y` is greater than `z`? From looking at the code! We can tell that `x` and `y` are given the same value (6), and that `y`'s value of 6 is bigger than `z`'s value of 4.

So far, we know we can pull in the values of each variable as premises, like this:

```text
import org.sireum.logika._

val x: Z = 6
val y: Z = 6
val z: Z = 4

l"""{
    1. x == 6           premise
    2. y == 6           premise
    3. z == 4           premise

    //how to say that x and y are equal, and that y is bigger than z?
}"""

assert (x == y & y > z)
```

But we don't yet know how to justify claims that describe how variables compare to one another. We will learn how to do that with the `algebra` and `subst` rules in section 8.4.

### Using conditional operators

Notice that the program above used an `&` symbol for an AND operator in an assert statement. Because asserts are part of the program and not part of a proof block, they will use the same conditional operators as in Logika programs. Here is a summary:

| Meaning | Operator in proofs | Operator in Logika programs/assumes/asserts |
| --- | --- | --- |
| p AND q | `p ∧ q` | `p & q` |
| p OR q | `p ∨ q` | `p \| q` |
| NOT p | `¬p`| `!p` |
| p IMPLIES q | `p → q` | not available |

If we wanted to write an assert statement that would be true when some variable `p` was even and/or was a positive two-digit number, we could say:

```text
assert(p % 2 == 0 | (p > 9 & p < 100))
```

The implies operator is not available in assert statements. However, we can make use of one of the equivalences we learned about in section 3.4: that `p → q` is equivalent to `¬ p ∨ q`. So if we wanted to assert that if `p` was positive, then `q` was equal to `p`, then we could write:

```text
//expressing the implicaton ((p > 0) → (q == p))
assert(!(p > 0) | (q == p))
```

Or, equivalently, we could write:

```text
assert((p <= 0) | (q == p))
```

## Assume statement

An *assume* statement in Logika uses the syntax `assume(expression)`. If the expression is satisfiable, then we can use `expression` as a premise in the following Logika proof block.

### Assume example

For example:

```text
var a: Z = readInt()
assume (a > 0)

l"""{
    1. a > 0            premise
}"""
```

Assume statements are almost always used to make assumptions about user input. Perhaps our program only works correctly for certain values of input. If we can assume that the user really did enter acceptable values, then we can use that information (by pulling it in as a premise in the next logic block) to prove the correctness of the program based on that assumption.

### Assumes vs. wrapping if-statements

Toy programs often use assume in lieu of wrapping code in an if statement. The following two examples are equivalent:

```text
import org.sireum.logika._

var a : Z = readInt()
assume (a != 0)

l"""{
    1. a != 0  premise
}"""

var b: Z = 20 / a
```

```text
//...is equivalent to:

import org.sireum.logika._

var a : Z = readInt()
var b : Z = 0

if (a != 0) {
    b = 20 / a
}
```

These examples also demonstrate a requirement when we use the division operator in Logika programs, we must first demonstrate that we are not dividing by zero.

### Unsatisfiable assume

You will see an error in Logika if your assume statement is not satisfiable. For example:

```text
import org.sireum.logika._

var a: Z = readInt()

assume(a > 0)
assume (a == 0)
```

If you try verifying this program, you will get a Logika error on the second `assume` statement. This is because we already assumed that `a` was greater than 0, and it is not possible for `a` to also be equal to 0.


