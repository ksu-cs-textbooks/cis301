---
title: "Nested Quantifiers"
pre: "6.3. "
weight: 72
date: 2018-08-24T10:53:26-05:00
---

Our examples so far have included propositions with single quantifiers. This section will discuss how to prove sequents that use nested quantifers. We will see that the approach is the same as before, but that we must take caution to process the quantifiers in the correct order. Recall that quantifier precedence is from right to left (i.e., from the outside in), so that `∀ x ∀ y P(x, y)` is equivalent to `∀ x (∀ y P(x, y))`.

//<------------------COME UP WITH DIFFERENT EXAMPLE 1!!!!!------------------->

## Example 1

Suppose we wish to prove the following sequent:

```text
∀ x ∀ y P(x, y) ⊢ ∀ x ∀ y P(y, x)
```

Since we wish to prove a for-all statement of the form `∀ y (SOMETHING)`, then we know we must start with our for all introduction template:

```text
∀ x ∀ y P(x, y) ⊢ ∀ y ∀ x P(y, x)
{
    1. ∀ x ∀ y P(x, y)              premise
    2. {
        3. a 
        
        //need: ∀ x P(a, x)
    }
    //want to use ∀i to conclude ∀ y ∀ x P(y, x)
}
```
But now we see that we want to prove ANOTHER for-all statement, `∀ x P(a, x)`. So we again use our for all introduction strategy in a nested subproof:

```text
∀ x ∀ y P(x, y) ⊢ ∀ y ∀ x P(y, x)
{
    1. ∀ x ∀ y P(x, y)              premise
    2. {
        3. a 
        4. {
            5. b

            //need: P(a, b)
        }
        //want to use ∀i to conclude ∀ x P(a, x)

        //need: ∀ x P(a, x)
    }
    //want to use ∀i to conclude ∀ y ∀ x P(y, x)
}
```

Now, in subproof 4, we see that we must use `∀e` on our premise (`∀ x ∀ y P(x, y)`) to work towards our goal of `P(a, b)`. We have two available individuals -- `a` and `b`. When we use `∀e`, we must eliminate the OUTER (top-level) quantifier and its variable. In the case of `∀ x ∀ y P(x, y)`, we see that we must eliminate the `∀ x`. Since the `x` is the first parameter in `P(x, y)`, and since we are hoping to reach `P(a, b)` by the end of subproof 4, we can see that we need to plug in the `a` for the `x` so that it will be in the desired position:

```text
∀ x ∀ y P(x, y) ⊢ ∀ y ∀ x P(y, x)
{
    1. ∀ x ∀ y P(x, y)              premise
    2. {
        3. a 
        4. {
            5. b
            6. ∀ y P(a, y)          ∀e 1 a

            //need: P(a, b)
        }
        //want to use ∀i to conclude ∀ x P(a, x)

        //need: ∀ x P(a, x)
    }
    //want to use ∀i to conclude ∀ y ∀ x P(y, x)
}
```

Note that on line 6, we could NOT have used `∀e` to eliminate the `∀ y` in `∀ x ∀ y P(x, y)`, as it was not the top-level operator. 

Next, we apply `∀e` again to `∀ y P(a, y)` to leave us with `P(a, b)`. All that remains at that point is to use `∀i` twice as planned to wrap up the two subproofs. Here is the completed proof:

```text
∀ x ∀ y P(x, y) ⊢ ∀ y ∀ x P(y, x)
{
    1. ∀ x ∀ y P(x, y)              premise
    2. {
        3. a 
        4. {
            5. b
            6. ∀ y P(a, y)          ∀e 1 a
            7. P(a, b)              ∀e 6 b

            //need: P(a, b)
        }
        //want to use ∀i to conclude ∀ x P(a, x)

        8. ∀ x P(a, x)              ∀i 4

        //need: ∀ x P(a, x)
    }
    //want to use ∀i to conclude ∀ y ∀ x P(y, x)

    9. ∀ y ∀ x P(y, x)              ∀i 2
}
```

## Example 2

Suppose we have the predicate `IsBossOf(x, y)` in the domain of people, which describes whether person `x` is the boss of person `y`. We wish to prove the following sequent:

```text
    (
        ∃((x: T) => ∀((y: T) => IsBossOf(x, y)))
    )
⊢
    (
        ∀((y: T) => ∃((x: T) => IsBossOf(x, y)))
    )
```

You can read the premise as "There is a person that is everyone's boss". From this statement, we are trying to prove the conclusion: "All people have a boss". Here is the completed proof:

```text
    (
        ∃((x: T) => ∀((y: T) => IsBossOf(x, y)))
    )
⊢
    (
        ∀((y: T) => ∃((x: T) => IsBossOf(x, y)))
    )

Proof(
    1 (     ∃((x: T) => ∀((y: T) => IsBossOf(x, y)))            )   by Premise,

    2 Let ( (a: T) => SubProof(
        3 Assume(   ∀((y: T) => IsBossOf(a, y)                  )

        4 Let ( (b: T) => SubProof(
            5 (         IsBossOf(a, b)                          )   by AllE[T](3),
            6 (         ∃((x: T) => IsBossOf(x, b))             )   by ExistsI[T](5)
        )),
        7 (   ∀((y: T) => ∃((x: T) => IsBossOf(x, y)))          )   by AllI[T](4)
    )),
    8 (   ∀((y: T) => ∃((x: T) => IsBossOf(x, y)))              )  by ExistsE[T](1, 2)
)
```

In the above proof, we let `a` be our made-up name for the boss-of-everyone. So, we have the assumption that `∀((y: T) => IsBossOf(a, y))`. Next, we let `b` be "anybody at all" who we might examine in the domain of people. The proof exposes that the boss of "anybody at all" in the domain must always be `a`. `AllI[T]` and then `ExistsE[T]` finish the proof.

Here is the proof worked again, with the subproofs swapped:

```text
    (
        ∃((x: T) => ∀((y: T) => IsBossOf(x, y)))
    )
⊢
    (
        ∀((y: T) => ∃((x: T) => IsBossOf(x, y)))
    )

Proof(
    1 (     ∃((x: T) => ∀((y: T) => IsBossOf(x, y)))            )   by Premise,

    2 Let ( (b: T) => SubProof(

        3 Let ( (a: T) => SubProof(
            4 Assume(   ∀((y: T) => IsBossOf(a, y))             ),
            5 (         IsBossOf(a, b)                          )   by AllE[T](4),
            6 (         ∃((x: T) => IsBossOf(x, b))             )   by ExistsI[T](5)
        )),
        7 (       ∃((x: T) => IsBossOf(x, b))                   )   by ExistsE[T](1, 3)
    )),
    8 (   ∀((y: T) => ∃((x: T) => IsBossOf(x, y)))              )  by AllI[T](2)
)
```

Can we prove the converse? That is, if everyone has a boss, then there is one boss who is the boss of everyone?

//<------------------STILL NEED TO FIX BELOW!!!!!------------------->

```text
∀ y ∃ x IsBossOf(x, y) ⊢ ∃ x ∀ y IsBossOf(x, y)
```

NO. We can try, but we get stuck:

```text
∀ y ∃ x IsBossOf(x, y) ⊢ ∃ x ∀ y IsBossOf(x, y)
{
    1. ∀ y ∃ x IsBossOf(x, y)           premise
    2. {
        3. a
        4. ∃ x IsBossOf(x, a)           ∀e 1 a
        5. {
            6. b IsBossOf(b, a)         assume
        }
        7. ∀ y isBoss(b, y)             ∀i 2  NO--THIS PROOF IS TRYING TO FINISH
                                          THE OUTER SUBPROOF WITHOUT FINISHING
                                          THE INNER ONE FIRST.

    ...can't finish
}
```

We see that the "block structure" of the proofs warns us when we are making invalid deductions.