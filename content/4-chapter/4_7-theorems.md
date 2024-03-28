---
title: "Theorems"
pre: "4.7. "
weight: 56
date: 2018-08-24T10:53:26-05:00
---

## Definition

A *theorem* in propositional logic is something that is always true with no need for premises. The truth table for a theorem is always a *tautology* -- it is true for any truth assignment.

To express a theorem as a sequent, we write:

```text
⊢ (theorem)
```

This shows that we are trying to prove our theorem with NO premises. Such a proof will always immediately begin with a subproof, as there are no premises to process.

## Law of the excluded middle, revisited

For example, the law of the excluded middle (LEM), `p ∨ ¬p`, is a theorem. We proved in section 4.5 that `p ∨ ¬p` is always true with no premises:

```text
⊢ (p ∨ ¬p)
    Proof(
        1 SubProof(
            2 Assume(  ¬(p ∨ ¬p)  ),

            3 SubProof(
                4 Assume(  p  ),
                5 (     p ∨ ¬ p         )   by OrI1(4),
                6 (     F               )   by NegE(5, 2)
            ),
            7 (     ¬p                  )   by NotI(3),
            8 (     p ∨ ¬p              )   by OrI2(7),
            9 (     F                   )   By NegE(8, 2)
        ),
        10 (        p ∨ ¬p              )   By PbC(1)
    )
```

We also see that the truth table for LEM is a tautology:

```text
      *
-------------
p # p ∨ ¬p
-------------
T #   T F
F #   T T
-------------
Tautology
```

## Another example

Suppose we wish to prove the following theorem of propositional logic:

```text
(p → q) → ((¬p → q) → q)
```

We would need to prove the sequent:

```text
⊢ ((p → q) → ((¬p → q) → q))
```

We see that the top-level operator of what we are trying to prove is an implies operator. So, we begin our proof using the strategy for implies introduction:

```text
⊢ ((p → q) → ((¬p → q) → q))
    Proof(
        1 SubProof(
            2 Assume(  p → q  ),

            //goal: reach (¬p → q) → q
        ),
        //use ImplyI to conclude (p → q) → ((¬p → q) → q)
    )
```

Inside subproof 1, we are trying to prove `(¬p → q) → q`. The top-level operator of that statement is an implies, so we nest another subproof with the goal of using implies introduction:

```text
⊢ ((p → q) → ((¬p → q) → q))
    Proof(
        1 SubProof(
            2 Assume(  p → q  ),

            3 SubProof(
                4 Assume(  ¬p → q  ),

                //goal: reach q
            )
            
            //use ImplyI to conclude (¬p → q) → q

            //goal: reach (¬p → q) → q
        ),
        //use ImplyI to conclude (p → q) → ((¬p → q) → q)
    )
```

Now we must prove `q` in subproof 3. We have available propositions `p → q` and `¬p → q` -- we can see that if we had LEM (`p ∨ ¬p`) available, then we could use OR elimination to get our `q` in both cases. We insert the LEM proof into subproof 3:

```text
⊢ ((p → q) → ((¬p → q) → q))
    Proof(
        1 SubProof(
            2 Assume(  p → q  ),

            3 SubProof(
                4 Assume(  ¬p → q  ),

                //Begin LEM proof, p ∨ ¬p
                5 SubProof(
                    6 Assume(  ¬(p ∨ ¬p)  ),

                    7 SubProof(
                        8 Assume(  p  ),
                        9 (     p ∨ ¬ p             )   by OrI1(8),
                        10 (    F                   )   by NegE(9, 6)
                    ),
                    11 (        ¬p                  )   by NotI(7),
                    12 (        p ∨ ¬p              )   by OrI2(11),
                    13 (        F                   )   By NegE(12, 4)
                ),
                14 (        p ∨ ¬p                  )   By PbC(5)

                //End LEM proof for p ∨ ¬p

                //goal: reach q
            )
            
            //use ImplyI to conclude (¬p → q) → q

            //goal: reach (¬p → q) → q
        ),
        //use ImplyI to conclude (p → q) → ((¬p → q) → q)
    )
```

Finally, we do OR elimination with `p ∨ ¬p` and tie together the rest of the proof:

```text
⊢ ((p → q) → ((¬p → q) → q))
    Proof(
        1 SubProof(
            2 Assume(  p → q  ),

            3 SubProof(
                4 Assume(  ¬p → q  ),

                //Begin LEM proof, p ∨ ¬p
                5 SubProof(
                    6 Assume(  ¬(p ∨ ¬p)  ),

                    7 SubProof(
                        8 Assume(  p  ),
                        9 (     p ∨ ¬ p             )   by OrI1(8),
                        10 (    F                   )   by NegE(9, 6)
                    ),
                    11 (        ¬p                  )   by NotI(7),
                    12 (        p ∨ ¬p              )   by OrI2(11),
                    13 (        F                   )   By NegE(12, 4)
                ),
                14 (        p ∨ ¬p                  )   By PbC(5),

                //End LEM proof for p ∨ ¬p

                //use OR elimination on p ∨ ¬p, try to reach q
                15 SubProof(
                    16 Assume(  p  ),
                    17 (        q               )   By ImplyE(2, 16)
                ),
                18 SubProof(
                    19 Assume(  ¬p  ),
                    20 (        q               )   By ImplyE(4, 19)
                ),
                21 (        q                   )   By OrE(14, 15, 18)

                //goal: reach q
            )
            
            //use ImplyI to conclude (¬p → q) → q
            22 (        (¬p → q) → q            )   By ImplyI(3)

            //goal: reach (¬p → q) → q
        ),
        //use ImplyI to conclude (p → q) → ((¬p → q) → q)

        23 (        (p → q) → ((¬p → q) → q)    )   By ImplyI(1)
    )
```

If we complete a truth table for `(p → q) → ((¬p → q) → q)`, we also see that it is a tautology:

```text
               *
-----------------------------------
p q # (p →: q) →: ((¬p →: q) →: q)
-----------------------------------
T T #    T     T    F  T     T
T F #    F     T    F  T     F
F T #    T     T    T  T     T
F F #    T     T    T  F     T
----------------------------------
Tautology
```