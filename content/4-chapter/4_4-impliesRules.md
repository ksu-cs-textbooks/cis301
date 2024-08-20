---
title: "Implies Rules"
pre: "4.4. "
weight: 53
date: 2018-08-24T10:53:26-05:00
---

In this section, we will see the deduction rules for the implies operator.

Note that in our Logika proofs, the implies operator is typed as `__>:` but is rendered as `→`.

## Implies elimination

Remember that `→` is a kind of logical "if-then". Here, we understand `p → q` to mean that `p` holds knowledge sufficient to deduce `q` – so, whenever `p` is proved to be a fact, then `p → q` enables `q` to be proved a fact, too. This is the implies elimination rule, `ImplyE`, and we can formalize it like this:

```text
            P → Q    P
 ImplyE :  ------------
                Q
```

Here is a simple example of a proof that shows the syntax of the `ImplyE` rule:

```text
(a, a → b) ⊢ (b)
    Proof(
        1 (     a          )   by Premise,
        2 (     a → b      )   by Premise,
        3 (     b          )   by ImplyE(2, 1)
    )
```

Note that when we use `ImplyE`, we first list the line number of the implies statement, and then list the line number that contains the left side of that implies statement. The `ImplyE` allows us to claim the right side of that implies statement. 

## Implies introduction

The idea behind the next deduction rule, implies introduction, is that we would be introducing a new implies statement of the form `P → Q`. In order to do this, we must be able to show our logical "if-then" -- that IF `P` exists, THEN we promise that `Q` will also exist. We do this by way of a subproof where we assume `P`. If we can reach `Q` by the end of that subproof, we will have shown that anytime `P` is true, then `Q` is also true. We will be able to close the subproof by introducing `P → Q` with the `ImplyI` rule. We can formalize the rule like this:

```text
           SubProof(
              Assume(  P  ),
              ...
              Q
           ),
ImplyI : -------------- 
              P → Q 
```

Here is a simple example of a proof that shows the syntax of the `ImplyI` rule:

```text
(a → b, b → c) ⊢ (a → c)
    Proof(
        1 (     a → b           )   by Premise,
        2 (     b → c           )   by Premise,
        3 SubProof(
            //we want to prove a → c, so we start by assuming a

            4 Assume(  a  ),
            5 (     b           )   by ImplyE(1, 4),
            6 (     c           )   by ImplyE(2, 5)

            //...and try to end with c
        ),
        //then we can conclude that anytime a is true, then c is also true

        7 (     a → c           )   by ImplyI(3)
    )
```
Note that when we use `ImplyI`, we list the line number of the subproof we just finished. We must have started that subproof by assuming the left side of the implies statement we are introducing, and ended that subproof with the right side of the implies statement we are introducing.


## Example 1

Suppose we want to prove the following sequent:

```text
(p → (q → r)) ⊢ ((q ∧ p) → r)
```

We start by listing our premise:

```text
(p → (q → r)) ⊢ ((q ∧ p) → r)
    Proof(
        1 (     p → (q → r)        )   by Premise,
        ...
    )
```

We can't extract any information from the premise, so we shift to examining our conclusion. The top-level operator of our conclusion is an implies statement, so this tells us that we will need to use the `ImplyI` rule. We want to prove `(q ∧ p) → r`, so we need to show that whenever `q ∧ p` is true, then `r` is also true. We open a subproof and assume the left side of our goal implies statement (`q ∧ p`). If we can reach `r` by the end of the subproof, then we can use `ImplyI` to conclude `(q ∧ p) → r`:

```text
(p → (q → r)) ⊢ ((q ∧ p) → r)
    Proof(
        1 (     p → (q → r)        )   by Premise,

        2 SubProof(
            3 Assume(  q ∧ p  ),

            //goal: get to r
        ),
        //use ImplyI to conclude (q ∧ p) → r
    )
```

Now we can complete the proof:

```text
(p → (q → r)) ⊢ ((q ∧ p) → r)
    Proof(
        1 (     p → (q → r)        )   by Premise,

        2 SubProof(
            3 Assume(  q ∧ p  ),
            4 (     q              )   by AndI1(3),
            5 (     p              )   by AndI2(3),
            6 (     q → r          )   by ImplyE(1, 5),
            7 (     r              )   by ImplyE(6, 4)

            //goal: get to r
        ),

        //use ImplyI to conclude (q ∧ p) → r
        8 (     (q ∧ p) → r        )   by ImplyI(2)
    )
```

## Example 2

Suppose we want to prove the following sequent:

```text
(p → (q → r) ⊢ (p → q) → (p → r))
```

We see that we will have no information to extract from the premises. The top-level operator is an implies statement, so we start a subproof to introduce that implies statement. In our subproof, we will assume the left side of our goal implies statement (`p → q`) and will try to reach the right side of our goal (`p → r`):

```text
(p → (q → r) ⊢ (p → q) → (p → r))
    Proof(
        1 (     p → (q → r)            )   by Premise,

        2 SubProof(
            3 Assume(  p → q  ),
            
            //goal: get to p → r
        ),
        //use ImplyI to conclude (p → q) → (p → r)
    )
```

We can see that our goal is to reach `p → r` in our subproof -- so we see that we need to introduce another implies statement. This tells us that we need to nest another subproof -- in this one, we'll assume the left side of our *current* goal implies statement (`p`), and then try to reach the right side of that current goal (`r`). Then, we'd be able to finish that inner subproof by using `ImplyI` to conclude `p → r`:

```text
(p → (q → r) ⊢ (p → q) → (p → r))
    Proof(
        1 (     p → (q → r)            )   by Premise,

        2 SubProof(
            3 Assume(  p → q  ),

            4 SubProof(
                5 Assume(  p  ),

                //goal: get to r
            ),
            //use ImplyI to conclude p → r
            
            //goal: get to p → r
        ),
        //use ImplyI to conclude (p → q) → (p → r)
    )
```

Now we can complete the proof:

```text
(p → (q → r) ⊢ (p → q) → (p → r))
    Proof(
        1 (     p → (q → r)             )   by Premise,

        2 SubProof(
            3 Assume(  p → q  ),

            4 SubProof(
                5 Assume(  p  ),
                6 (     q               )   by ImplyE(3, 5),
                7 (     q → r           )   by ImplyE(1, 5),
                8 (     r               )   by ImplyE(7, 6)

                //goal: get to r
            ),
            //use ImplyI to conclude p → r

            9 (     p → r               )   by ImplyI(4)
            
            //goal: get to p → r
        ),
        //use ImplyI to conclude (p → q) → (p → r)

        10 (    (p → q) → (p → r)       )   by ImplyI(2)
    )
```

## Example 3

Here is one more example, where we see we can nest an `ImplyI` subproof and a `OrE` subproof:

```text
(p → r, q → r) ⊢ ((p ∨ q) → r)
    Proof(
        1 (     p → r              )   by Premise,
        2 (     q → r              )   by Premise,
        3 SubProof(
            //assume p ∨ q, try to get to r 

            4 Assume(  p ∨ q  ),
            
            //nested subproof for OR elimination on p ∨ q
            //try to get to r in both cases
            
            5 SubProof(
                6 Assume(  p  ),
                7 (     r          )   by ImplyE(1, 6)
            ),
            8 SubProof(
                9 Assume(  q  ),
                10 (    r          )   by ImplyE(2, 9)
            ),
            11 (    r              )   by OrE(4, 5, 8)

            //goal: get to r
        ),

        //use ImplyI to conclude (p ∨ q) → r
        12 (    (p ∨ q) → r        )   by ImplyI(3)
    )
```