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
    (
        ∀((x: T) => ∀((y: T) => (P(x, y) → Q(x, y)))),
        ∀((x: T) => ∀((y: T) => P(x, y)))
    )
⊢
    (
        ∀((x: T) => ∀((y: T) => Q(x, y)))
    )
```

Since we wish to prove a for-all statement, `∀((x: T) => (SOMETHING)`, we know we must start with our for all introduction template:

```text
    (
        ∀((x: T) => ∀((y: T) => (P(x, y) → Q(x, y)))),
        ∀((x: T) => ∀((y: T) => P(x, y)))
    )
⊢
    (
        ∀((x: T) => ∀((y: T) => Q(x, y)))
    )
Proof(
    1 (     ∀((x: T) => ∀((y: T) => (P(x, y) → Q(x, y))))       )   by Premise,
    2 (     ∀((x: T) => ∀((y: T) => P(x, y)))                   )   by Premise,
    
    3 Let (  (a: T)  => SubProof(

        //need: ∀((y: T) => Q(a, y))
    )),
    //want to use AllI[T] to conclude ∀((x: T) => ∀((y: T) => Q(x, y)))
)
```
But now we see that we want to prove ANOTHER for-all statement, `∀((y: T) => Q(a, y))`. So we again use our for all introduction strategy in a nested subproof:

```text
    (
        ∀((x: T) => ∀((y: T) => (P(x, y) → Q(x, y)))),
        ∀((x: T) => ∀((y: T) => P(x, y)))
    )
⊢
    (
        ∀((x: T) => ∀((y: T) => Q(x, y)))
    )
Proof(
    1 (     ∀((x: T) => ∀((y: T) => (P(x, y) → Q(x, y))))       )   by Premise,
    2 (     ∀((x: T) => ∀((y: T) => P(x, y)))                   )   by Premise,
    
    3 Let (  (a: T)  => SubProof(
        4 ( (b: T) => SubProof(

            //need: Q(a, b)
        )),
        //want to use AllI[T] to conclude ∀((y: T) => Q(a, y))

        //need: ∀((y: T) => Q(a, y))
    )),
    //want to use AllI[T] to conclude ∀((x: T) => ∀((y: T) => Q(x, y)))
)
```

Now, in subproof 4, we see that we must use `AllE[T]` on our both of our premises to work towards our goal of `Q(a, b)`. We have two available individuals -- `a` and `b`. When we use `AllE[T]`, we must eliminate the OUTER (top-level) quantifier and its variable. In the case of the premise `∀((x: T) => ∀((y: T) => (P(x, y) → Q(x, y))))`, we see that we must eliminate the `∀((x: T) ...)`. Since the `x` is the first parameter in `Q(x, y)`, and since we are hoping to reach `Q(a, b)` by the end of subproof 4, we can see that we need to plug in the `a` for the `x` so that it will be in the desired position. We make a similar substitution with `AllE[T]` on our second premise:

```text
    (
        ∀((x: T) => ∀((y: T) => (P(x, y) → Q(x, y)))),
        ∀((x: T) => ∀((y: T) => P(x, y)))
    )
⊢
    (
        ∀((x: T) => ∀((y: T) => Q(x, y)))
    )
Proof(
    1 (     ∀((x: T) => ∀((y: T) => (P(x, y) → Q(x, y))))       )   by Premise,
    2 (     ∀((x: T) => ∀((y: T) => P(x, y)))                   )   by Premise,
    
    3 Let (  (a: T)  => SubProof(
        4 ( (b: T) => SubProof(

            5 (     ∀(y: T) => (P(a, y) → Q(a, y)))             )   by AllE[T](1),
            6 (     ∀(y: T) => P(a, y)                          )   by AllE[T](2),

            //need: Q(a, b)
        )),
        //want to use AllI[T] to conclude ∀((y: T) => Q(a, y))

        //need: ∀((y: T) => Q(a, y))
    )),
    //want to use AllI[T] to conclude ∀((x: T) => ∀((y: T) => Q(x, y)))
)
```

Note that on line 5, we could NOT have used `AllE[T]` to eliminate the `∀(y: T)` in `∀((x: T) => ∀((y: T) => (P(x, y) → Q(x, y)))) `, as it was not the top-level operator. 

Next, we apply `AllE[T]` again to our results on lines 5 and 6, this time plugging in `b` for `y` in both cases. This leaves us with `P(a, b) → Q(a, b)` and `P(a, b)`. We can use implies elimination to reach our goal of `Q(a, b)`, and then all that remains ais to use `AllI[T]` twice as planned to wrap up the two subproofs. Here is the completed proof:

```text
    (
        ∀((x: T) => ∀((y: T) => (P(x, y) → Q(x, y)))),
        ∀((x: T) => ∀((y: T) => P(x, y)))
    )
⊢
    (
        ∀((x: T) => ∀((y: T) => Q(x, y)))
    )
Proof(
    1 (     ∀((x: T) => ∀((y: T) => (P(x, y) → Q(x, y))))       )   by Premise,
    2 (     ∀((x: T) => ∀((y: T) => P(x, y)))                   )   by Premise,
    
    3 Let (  (a: T)  => SubProof(
        4 ( (b: T) => SubProof(

            5 (     ∀(y: T) => (P(a, y) → Q(a, y))              )   by AllE[T](1),
            6 (     ∀(y: T) => P(a, y)                          )   by AllE[T](2),
            7 (     P(a, b) → Q(a, b)                           )   by AllE[T](5),
            8 (     P(a, b)                                     )   by ALlE[T](6),
            9 (     Q(a, b)                                     )   by ImplyE(7, 8)

            //need: Q(a, b)
        )),
        //want to use AllI[T] to conclude ∀((y: T) => Q(a, y))
        10 (    ∀((y: T) => Q(a, y))                            )   by AllI[T](4)

        //need: ∀((y: T) => Q(a, y))
    )),
    //want to use AllI[T] to conclude ∀((x: T) => ∀((y: T) => Q(x, y)))
    11 (    ∀((x: T) => ∀((y: T) => Q(x, y)))                   )   by AllI[T](3)
)
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

Can we prove the converse? That is, if everyone has a boss, then there is one boss who is the boss of everyone? NO. We can try, but we get stuck:

```text
    (
        ∀((y: T) => ∃((x: T) => IsBossOf(x, y)))
    )
⊢
    (
        ∃((x: T) => ∀((y: T) => IsBossOf(x, y)))
    )

Proof(
    1 (     ∀((y: T) => ∃((x: T) => IsBossOf(x, y)))            )   by Premise,

    2 Let ( (a: T) => SubProof(
        3 (     ∃((x: T) => IsBossOf(x, a))                     )   by AllE[T](1),
        
        4 Let ( (b: T) => SubProof(
            5 (     Assume (    IsBossOf(b, a)                  ),
        )),
        6 (     ∀((y: T) => IsBossOf(b, y))                     )   AllI[T](4),

        //STEP 6 IS INVALID -- we cannot refer to b after the end of the subproof
        //where it was introduced

    )),
    
    //...can't finish
)
```

We see that the "block structure" of the proofs warns us when we are making invalid deductions.