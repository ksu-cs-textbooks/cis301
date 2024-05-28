---
title: "Integer Division and Modulo"
pre: "8.6. "
weight: 95
date: 2018-08-24T10:53:26-05:00
---

We will see in this section that verifying programs with division and modulo requires extra care.

## Division

Recall that `Z` (int) is the only numeric type for Logika verification, so any division is integer division. This means something like `9/2` evaluates to 4, just as it would in Java or C#.

### Check for division by zero

Before doing division of the form `numerator/denominator`, either in a line of code or in a proof block, you must have have shown in a previous proof block that `denominator` is not 0. The easiest way to do this is to prove the claim: `denominator != 0`. You are even required to do this when dividing by a constant value that is obviously not zero.

For example, if we do:

```text
// #Sireum #Logika
//@Logika: --manual --background save
import org.sireum._
import org.sireum.justification._

var x: Z = 10 / 2
```

Then Logika will give us an error complaining we have not proved that the denominator is not zero. We must add the claim `2 != 0` as follows:

```text
// #Sireum #Logika
//@Logika: --manual --background save
import org.sireum._
import org.sireum.justification._


Deduce(
    1  (    2 != 0      )   by Algebra*()
)

var x: Z = 10 / 2
```

Note that our justification is just, `Algebra*()`, as we don't need any extra information to claim that 2 is not equal to 0.

We could have instead claimed that 2 > 0 (again using `Algebra*()`), which would also prove that the denominator was not 0. However, claims such as `2 > 1` would not be accepted as proof that the denominator wasn't 0 (at least in Logika's manual mode).

### Pitfalls

Be careful when making claims that involve division. For example, the following claim will not validate in Logika:

```text
Deduce(
    1  (    x == (x/3)*3    )   by Algebra*()
)
```

While the claim `x == (x/3)*3` is certainly true in math, it is not true with integer division. For example, if `x` is 7, then `(x/3)*3` is 6 -- so the two sides are not equal. In general, I recommend avoiding claims involving division if you can at all help it. Instead, try to find a way to express the same idea in a different way using multiplication.

## Modulo

Modulo (%) works the same way in Scala (and Logika) as it does in other programming languages. For example, `20 % 6` evaluates to 2.

### Modulo checks on denominator

Before using the modulo operator in the form `numerator % denominator`, either in a line of code or as a claim in a proof block, you must have previously established that `denominator` is not 0. This must be done even if the denominator is a literal value (like 2), and can be demonstrated in the same way as we did with division.

For example:

```text
Deduce(
    1  (    7 != 0      )   by Algebra*()
)

x = a % 7
```

### Example

Consider the following program:

```text
// #Sireum #Logika
//@Logika: --manual --background save
import org.sireum._
import org.sireum.justification._

var num: Z = Z.prompt("Enter positive number: ")

assume(num > 0)
val orig: Z = num

num = num * 2

assert(num % 2 == 0)

num = num + 2

assert(num % 2 == 0)

num = num/2 - 1

assert(num == orig)
```

It can often be handy to walk through a program with sample numbers before trying to prove its correctness:

- Suppose our input value, `num`, is 11
- `orig` is initialized to be 11 also
- `num` is multiplied by 2, and is 22
- It makes sense that `num` would be even, since any number times two is always even (and indeed, 22 is even)
- We add 2 to `num`, so it is now 24
- It makes sense that `num` would still be even, as it was even before and we added 2 (another even number) to it. Indeed, 24 is still even.
- We update `num` by dividing it by 2 and subtracting 1, so `num` is now back to its original value of 11 (the same as `orig`). This step "undoes" the changes we have made to `num` -- looking at the code, we can see that the final value of `num` is `orig*2 + 2`, so if we do `(orig*2 + 2) / 2 - 1`, we are left with `orig`.

Here is the completed verification, with comments at each step:

```text
// #Sireum #Logika
//@Logika: --manual --background save
import org.sireum._
import org.sireum.justification._

var num: Z = Z.prompt("Enter positive number: ")

assume(num > 0)
val orig: Z = num
num = num * 2

Deduce(
    1  (    num == Old(num) * 2     )   by Premise,         //we updated num to be its old value times 2
    2  (    orig == Old(num)        )   by Premise,         //orig equaled the old value of num (before our change)
    3  (    num == orig * 2         )   by Algebra*(1, 2),  //express the new value of num without referring to "Old"
    4  (    2 != 0                  )   by Algebra*(),      //needed to use modulo in the assert
    5  (    num % 2 == 0            )   by Algebra*(1)      //we have showed num is now even (needed for next assert)
)

assert(num % 2 == 0)

num = num + 2 

Deduce(
    1  (    num == Old(num) + 2     )   by Premise,         //we updated num to be its old value plus 2
    2  (    Old(num) % 2 == 0       )   by Premise,         //from line 5 in previous block, but num has since changed
    3  (    num % 2 == 0            )   by Algebra*(1, 2),  //we have showed num is still even (needed for next assert)
    4  (    Old(num) == orig * 2    )   by Premise,         //from line 3 in block above, but num has since changed
    5  (    num - 2 == orig * 2     )   by Algebra*(1, 4)   //express new value of num without using "Old"
)

assert(num % 2 == 0)

//we have already established that 2!= 0, which is needed to divide by 2

num = num/2 - 1

Deduce(
    1  (    num == Old(num) / 2 - 1         )   by Premise,         //we updated num to be its old value divided by 2 minus 1
    2  (    Old(num) - 2 == orig * 2        )   by Premise,         //from line 7 in previous block, but num has since changed
    3  (    Old(num) == orig * 2 + 2        )   by Algebra*(2),
    4  (    num == (orig * 2 + 2) / 2 - 1   )   by Algebra*(1, 3),  //express new value of num without using "Old"
    5  (    num == orig + 1 - 1             )   by Algebra*(4),
    6  (    num == orig                     )   by Algebra*(5)      //we have showed num is back to being orig (needed for last assert)
                                                                    //could have skipped straight here with "Algebra*(1,2)"
)

assert(num == orig)
```

Sometimes it can be tricky to figure out what to do at each step in order to get assert statements to pass. If you are unsure what to do, I recommend:

- Walk through the program several times with sample numbers, keeping track of changes to variables. Why do the assert statements make sense to you? Convince yourself that they are valid claims before you worry about the formal verification.

- Add a proof block after each variable mutation. Work from the top down:
    - Write a premise for every variable assignment and assume statement since the previous proof block.
    - Find all claims in the proof block just before you that do not use an "Old" reference -- pull each claim into your current block, using an "Old" reference as needed for the most recently changed variable.
    - Manipulate your claims that use an "Old" reference until you have statements that capture the current value of the recently changed variable that do not reference "Old"
    - If your next statement is an assert, manipulate your claims until you have exactly the claim in the assert.
    - If any claims 

- Add a proof block before each use of division (`numerator / denominator`) and modulus (`numerator % denominator`). Pull in claims from previous blocks as described above to help you show the claim `denominator != 0`. If you can, avoid using division in proof block claims.