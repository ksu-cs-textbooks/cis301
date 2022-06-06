---
title: "Integer Division and Modulo"
pre: "8.6. "
weight: 95
date: 2018-08-24T10:53:26-05:00
---

Working with division and modulo requires extra care, as Logika is finicky about both.

## Division

Recall that `Z` (int) is the only numeric type in Logika, so any division is integer division. This means something like `9/2` evaluates to 4, just as it would in Java or C#.

### Check for division by zero

Before doing division of the form `numerator/denominator`, either in a line of code or in a logic block, you must have a line in a previous logic block that states: `denominator != 0`. Other forms, such as `denominator > 0` or `0 != denominator`, will not work. You are even required to do this when dividing by a constant value that is obviously not zero.

For example, if we do:

```text
import org.sireum.logika._

var x: Z = 10 / 2
```

Then Logika will give us an error complaining we have not proved that the denominator is not zero. We must add the claim `2 != 0` as follows:

```text
import org.sireum.logika._

l"""{
    1. 2 != 0           algebra
}"""

var x: Z = 10 / 2
```

Note that our justification is just, `algebra`, as we don't need any extra information to claim that 2 is not equal to 0.

### Pitfalls

Be careful when making claims that involve division. For example, the following claim will not validate in Logika:

```text
l"""{
    1. x == (x/3)*3         algebra     //NO!
}"""
```

While the claim `x == (x/3)*3` is certainly true in math, it is not true with integer division. For example, if `x` is 7, then `(x/3)*3` is 6 -- so the two sides are not equal. In general, I recommend avoiding claims involving division if you can at all help it. Instead, try to find a way to express the same idea in a different way using multiplication.

## Modulo

Modulo (%) works the same way in Logika as it does in other programming languages. For example, `20 % 6` evaluates to 2.

### Modulo checks on numerator and denominator

Before using the modulo operator in the form `numerator % denominator`, either in a line of code or as a claim in a logic block, you must have EXACTLY these claims in the previous logic block:

- `numerator >= 0`
- `denominator > 0`

For example:

```text
...

l"""{
    1. 2 > 0           algebra
    2. a >= 0          (some justification)
}"""

x = a % 2

...
```

### Example

Consider the following Logika program:

```text
import org.sireum.logika._

var num: Z = readInt("Enter positive number: ")

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
import org.sireum.logika._

var num: Z = readInt("Enter positive number: ")

assume(num > 0)
val orig: Z = num
num = num * 2

l"""{
    1. num == num_old * 2           premise     //we updated num to be its old value times 2
    2. orig == num_old              premise     //orig equaled the old value of num (before our change)
    3. num == orig * 2              algebra 1 2 //express the new value of num without referring to "_old"
    4. 2 > 0                        algebra     //needed to use modulo in step 7
    5. num_old > 0                  premise     //we assumed the old value of num (before its change) was > 0
    6. num >= 0                     algebra 1 5 //needed to use modulo in step 7
    7. num % 2 == 0                 algebra 1   //we have showed num is now even (needed for next assert)
}"""

assert(num % 2 == 0)

num = num + 2 

l"""{
    1. num == num_old + 2               premise     //we updated num to be its old value plus 2
    2. num_old >= 0                     premise     //from line 6 in previous block, but num has since changed
    3. num_old % 2 == 0                 premise     //from line 7 in previous block, but num has since changed

    //we know 2 > 0 from previous block - don't need to restate here

    4. num >= 0                         algebra 1 2 //needed to use modulo in step 5 (need to redo since num has changed)
    5. num % 2 == 0                     algebra 1 3 //we have showed num is still even (needed for next assert)
    6. num_old == orig * 2              premise     //from line 3 in block above, but num has since changed
    7. num - 2 == orig * 2              algebra 1 6 //express new value of num without using "_old"
}"""

assert(num % 2 == 0)

l"""{
    1. 2 != 0                       algebra     //needed for dividing by 2
}"""

num = num/2 - 1

l"""{
    1. num == num_old/2 - 1             premise     //we updated num to be its old value divided by 2 minus 1
    2. num_old - 2 == orig * 2          premise     //from line 7 in previous block, but num has since changed
    3. num_old == orig * 2 + 2          algebra 2
    4. num == (orig * 2 + 2)/2 - 1      algebra 1 3 //express new value of num without using "_old"
    5. num == orig + 1 - 1              algebra 4
    6. num == orig                      algebra 5   //we have showed num is back to being orig (needed for last assert)
                                                    //could have skipped straight here with "algebra 1 2"

assert(num == orig)
```

Sometimes it can be tricky to figure out what to do at each step in order to get assert statements to pass. If you are unsure what to do, I recommend:

- Walk through the program several times with sample numbers, keeping track of changes to variables. Why do the assert statements make sense to you? Convince yourself that they are valid claims before you worry about the formal verification.

- Add a logic block after each variable mutation. Work from the top down:
    - Write a premise for every variable assignment and assume statement since the previous logic block.
    - Find all claims in the logic block just before you that do not use an "_old" reference -- pull each claim into your current block, using an "_old" reference as needed for the most recently changed variable.
    - Manipulate your claims that use an "_old" reference until you have statements that capture the current value of the recently changed variable that do not reference "_old"
    - If your next statement is an assert, manipulate your claims until you have exactly the claim in the assert.
    - If any claims 

- Add a logic block before each use of division (`numerator / denominator`) and modulus (`numerator % denominator`). Pull in claims from previous blocks as described above to help you show what is needed about `numerator` and `denominator`:
    - For division, build to the claim `denominator != 0`
    - For modulo, build to the claims `numerator >= 0` and `denominator > 0`
    - If you can, avoid using division in logic block claims