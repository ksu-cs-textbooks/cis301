---
title: "Assert and Assume"
pre: "8.3. "
weight: 92
date: 2018-08-24T10:53:26-05:00
---

## Assert statements

An *assert* statement in Scala uses the syntax `assert(expression)`, where `expression` is of type `bool`. The assert passes if the expression is true, and throws an error if the expression is false.

Each time we use an assert, the Logika proof checker will look to see if we have logically justified the expression being asserted. If we have, we will get a purple check mark indicate that the assert statement has been satisified. If we have not, we will get a red error indicating that the assertion has not been proven.

Note that we are using these assert statements differently than assert statements in languages like Java and C# -- in those languages, the code in the assert statement (which often includes test method calls) is actually run, and the assert statement checks whether those methods are returning the expected values. In Logika, assert statements only pass if we have previously PROVED what we are claiming -- the code is never executed. 

### Example

Consider the following program:

```text
// #Sireum #Logika
//@Logika: --manual --background save
import org.sireum._
import org.sireum.justification._
import org.sireum.justification.natded.prop._

val x: Z = 6
val y: Z = 6
val z: Z = 4

assert (x == y & y > z)
```

While we can tell that `x` equals `y` and that `y` is greater than `z`, this code would fail in Logika (at least in its manual mode, which is what we are using now). In order for an assert to pass, we must have already proved EXACTLY the statement in the assert.

Why do WE know that `x` equals `y` and that `y` is greater than `z`? From looking at the code! We can tell that `x` and `y` are given the same value (6), and that `y`'s value of 6 is bigger than `z`'s value of 4.

So far, we know we can pull in the values of each variable as premises, like this:

```text
// #Sireum #Logika
//@Logika: --manual --background save
import org.sireum._
import org.sireum.justification._
import org.sireum.justification.natded.prop._

val x: Z = 6
val y: Z = 6
val z: Z = 4

Deduce(
    1  (x == 6) by Premise,
    2  (y == 6) by Premise,
    3  (z == 4) by Premise,
  
    //how to say that x and y are equal, and that y is bigger than z?

)
assert(x == y ∧ y > z)
```

But we don't yet know how to justify claims that describe how variables compare to one another. We will learn how to do that with the `Algebra` and `Subst` rules in section 8.4.

### Using conditional operators

Notice that the program above used an `∧` symbol for an AND operator in an assert statement. As with our propositional logic proofs, if we want to use a `∧` operator then we type the symbol `&`. This `&` is automatically changed to a `∧` when we view our program, just as it was in our deduction proofs. Similarly, we type `!` for NOT in asserts (which displays as `¬`) and `|` for OR (which displays as `∨`). However, we are not able to use an implies operator in an assert statement as it is not part of the Scala language (and our asserts are part of the program and not just a proof element).

If we wanted to write an assert statement that would be true when some variable `p` was even and/or was a positive two-digit number, we could say:

```text
assert(p % 2 == 0 ∨ (p > 9 ∧ p < 100))
```

As was mentioned above, the implies operator is not available in assert statements. However, we can make use of one of the equivalences we learned about in section 3.4: that `p → q` is equivalent to `¬p ∨ q`. So if we wanted to assert that if `p` was positive, then `q` was equal to `p`, then we could write:

```text
//expressing the implicaton ((p > 0) → (q == p))
assert(¬(p > 0) ∨ (q == p))
```

Or, equivalently, we could write:

```text
assert((p <= 0) ∨ (q == p))
```

## Assume statement

An *assume* statement in Scala uses the syntax `assume(expression)`. If the expression is satisfiable, then we can use `expression` as a premise in the following Logika proof block.

### Assume example

For example:

```text
var a: Z = Z.read()
assume (a > 0)

Deduce(
    1 (a > 0)   by Premise
)
```

Assume statements are almost always used to make assumptions about user input. Perhaps our program only works correctly for certain values of input. If we can assume that the user really did enter acceptable values, then we can use that information (by pulling it in as a premise in the next proof block) to prove the correctness of the program based on that assumption.

### Assumes vs. wrapping if-statements

Toy programs often use assume in lieu of wrapping code in an if statement. The following two examples are equivalent:

```text
// #Sireum #Logika
//@Logika: --manual --background save
import org.sireum._
import org.sireum.justification._

var a : Z = Z.read()
assume (a != 0)

Deduce(
  1  (a != 0) by Premise
)

var b: Z = 20 / a
```

```text
//...is equivalent to:

// #Sireum #Logika
//@Logika: --manual --background save
import org.sireum._
import org.sireum.justification._


var a : Z = Z.read()
var b : Z = 0

if (a != 0) {
    b = 20 / a
}
```

These examples also demonstrate a requirement when we use the division operator in Logika programs, we must first demonstrate that we are not dividing by zero.

