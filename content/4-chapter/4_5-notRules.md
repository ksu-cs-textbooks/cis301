---
title: "Negation Rules"
pre: "4.5. "
weight: 54
date: 2018-08-24T10:53:26-05:00
---

In this section, we will see the deduction rules for the NOT operator. In this section, we will introduce the notion of a *contradiction*, which occurs when, for some proposition `P`, we have proved both the facts `P` and `¬ P`. This indicates that we are in an impossible situation, and often means that we have made a bad previous assumption. In Logika, we use an `F` ("false") as a claim to indiciate that we've reached a contradiction. In other texts, you sometime see the symbol `⊥` (which means "bottom operator") for a contradiction.

## Negation elimination

The negation elimination rule allows you to claim that you have reached a contradiction. We can formalize the rule like this:

```text
        P   ¬P
NegE : -----------
           F
```

Here is a simple example of a proof that shows the syntax of the `NegE rule:

```text
(q, ¬q) ⊢ (F)
    Proof(
        1 (     q       )   by Premise,
        2 (     ¬q      )   by Premise,
        3 (     F       )   by NegE(1, 2)
    )
```

We use the `NegE` rule when we have facts for both `P` and `¬P` for some proposition `P`. When we use the justification, we first list the line number of the claim for `P` (line 1, in our case) and then the line number of the claim for `¬P` (line 2, in our case).

Sometimes, the proposition `P` itself has a NOT operator. Consider this example:

```text
(¬q, ¬¬q) ⊢ (F)
    Proof(
        1 (     ¬q      )   by Premise,
        2 (     ¬¬q     )   by Premise,
        3 (     F       )   by NegE(1, 2)
    )
```

Here, our proposition `P` is the claim `¬q`, and our proposition that is of the form `¬P` is the claim `¬¬q`.

## Negation introduction

The negation introduction rule allows us to introduce a NOT operation. If assuming some proposition `P` leads to a contradiction, then we must have made a bad assumption -- `P` must NOT be true after all. We can then introduce the fact `¬P`. We can formalize the negation introduction rule like this:

```text
        SubProof(
            Assume ( P ),
            ...
            F
        ),
NotI : --------------
           ¬P
```

Here is a simple example of a proof that shows the syntax of the `NotI` rule:

```text
(p,  q → ¬p)  ⊢  (¬q)
    Proof(
        1 (     p           )       by Premise,
        2 (     q → ¬p      )       by Premise,

        3 SubProof(
            4 Assume (  q  ) ,
            5 (     ¬p      )       by ImplyE(2, 4),
            6 (     F       )       by NegE(1, 5)
        ),
        7 (     ¬q          )       by NegI(3)
    )
```

Note that the negation introduction rule involves a subproof -- if we wish to prove `¬P` for some proposition `P`, then we start a subproof where we assume `P`. If we are able to reach a contradiciton on the last line of that subproof, then we can use the `NegI` rule after the subproof ends to claim that our assumption was bad and that it is actually `¬P` that is true. When we use `¬i` as a justification, we list the line number corresponding to this subproof.

## Bottom elimination

There is a special law for reasoning forwards from an impossible situation — the ⊥e law — which says, in the case of a contradiction, everything becomes a fact. (That is, "if False is a fact, so is everything else ¬".) This rule is called "bottom elimination", and is written as `BottomE`. Here is a formalization of the rule:

```text
              F
BottomE :  ------  for any proposition, Q, at all
              Q
```

Here is a simple example of a proof that shows the syntax of the `BottomE` rule:

```text
(p, ¬p)  ⊢ (q)
Proof(
    1 (     p       )       by Premise,
    2 (     ¬p      )       by Premise,
    3 (     F       )       by NegE(1, 2),
    4 (     q       )       by BottomE(3)
)
```

Note that when we use `BottomE` as the justification, we list the line number of where we reached a contradiction (`F`).

The bottom elimination rule works well with case analysis, where we discover that one case is impossible. Here is a classic example:

```text
(p ∨ q, ¬p) ⊢ (q)
    Proof(
        1 (     p ∨ q       )   by Premise,
        2 (     ¬p          )   by Premise,

        3 SubProof(
            4 Assume(  p  ),
            5 (     F       )   by NegE(4, 2),
            6 (     q       )   by BottomE(5)
        ),
        7 SubProof(
            8 Assume(  q  )
        ),
        9 (     q           )   by OrE(1, 3, 7)
    )
```

Considering the premise, `p ∨ q`, we analyze our two cases by starting OR elimination. The first case, where `p` holds true, is impossible, because it causes a contradiction. The `BottomE`-rule lets us gracefully prove `q` in this "impossible case". (You can read lines 4-6 as saying, "in the case when `p` might hold true, there is a contradiction, and in such an impossible situation, we can deduce whatever we like, so we deduce `q` to finish this impossible case".)

The second case, that `q` holds true, is the only realistic case, and it immediately yields the conclusion. The proof finishes the two-case analysis with the `OrE` rule.

## Proof by contradiction

The proof by contraction rule, `PbC`, says that when assuming `¬P` leads to a contradiction for some proposition `P`, then we made a bad assumption and thus `P` must be true. It is very similar to the `NegI` rule, except `PbC` has us assuming `¬P` and eventually concluding `P`, while the `NegI` rule has us assuming `P` and eventually concluding `¬P`.

Here is a formalization of `PbC`:

```text
        SubProof(
             Assume( ¬P ),
             ...
             F   
        ),
PbC:   --------------------
               P
```

And here is an example that demonstrates the syntax of the rule:

```text
(¬¬p) ⊢ (p)
Proof(
    1 (     ¬¬p         )   by Premise,

    2 SubProof(
        3 Assume (  ¬p  ),
        4 (     F       )   by NegE(3, 1)
    ),
    5 (     p           )   by PbC(2)
)
```

When we use the `PbC` rule as a justification for a claim `P`, we cite the line number of the subproof where we assumed `¬P` and ended in a contradiction.

## Example 1

Suppose we want to prove the following sequent:

```text
(¬p ∧ ¬q) ⊢ (¬(p ∨ q))
```

We start by listing our premise, and extracting the two sides of the AND statement:

```text
(¬p ∧ ¬q) ⊢ (¬(p ∨ q))
    Proof(
        1 (     ¬p ∧ ¬q     )   by Premise,
        2 (     ¬p          )   by AndE1(1),
        3 (     ¬q          )   by AndE2(1),
        ...
    )
```

Next, we see that our conclusion has the form NOT (something), so this tells us that we will need to introduce a NOT (using the `NegI` rule). In fact, ANY time we wish to prove a proposition of the form NOT (something), we will always use the `NegI` rule. (We will discuss proof strategies in detail in the next section.) Since we want to prove `¬(p ∨ q)`, then we open a subproof where we assume `p ∨ q`. If we can end that subproof with a contradiction, then we can use `NeEgI` afterwards to conclude `¬(p ∨ q)`.

We know that we want this proof structure:

```text
(¬p ∧ ¬q) ⊢ (¬(p ∨ q))
    Proof(
        1 (     ¬p ∧ ¬q     )   by Premise,
        2 (     ¬p          )   by AndE1(1),
        3 (     ¬q          )   by AndE2(1),

        4 SubProof(
            5 Assume (  p ∨ q  ),
            
            //want to reach a contradiction
        ),
        //will use NegI to conclude ¬(p ∨ q)
    )
```

We know we must reach a contradiction in our subproof. We see that we have claims `¬p`, `¬q`, and `p ∨ q `. Since at least one of `p ∨ q ` is true, and since either one would yield a contradiction with one of `¬p` or `¬q`, then we start on OR elimination:

```text
(¬p ∧ ¬q) ⊢ (¬(p ∨ q))
    Proof(
        1 (     ¬p ∧ ¬q     )   by Premise,
        2 (     ¬p          )   by AndE1(1),
        3 (     ¬q          )   by AndE2(1),

        4 SubProof(
            5 Assume (  p ∨ q  ),
            
            //OR elimination subproofs on p ∨ q
            6 SubProof(
                7 Assume(  p  ),
                8 (     F       )   by NegE(7, 2)
            ),
            9 SubProof(
                10 Assume(  q  ),
                11 (    F       )   by NegE(10, 3)
            ),
            
            //use OrE rule to tie together subproofs

            //want to reach a contradiction
        ),
        //will use NegI to conclude ¬(p ∨ q)
    )
```

We see that both OR elimination subproofs ended with a contradiction (`F`). Just like any other use of `OrE`, we restate that common conclusion after the two subproofs. We knew at least one of `p` or `q` were true, and both ended in a contradiction -- so the contradiction holds no matter what:

```text
(¬p ∧ ¬q) ⊢ (¬(p ∨ q))
    Proof(
        1 (     ¬p ∧ ¬q     )   by Premise,
        2 (     ¬p          )   by AndE1(1),
        3 (     ¬q          )   by AndE2(1),

        4 SubProof(
            5 Assume (  p ∨ q  ),
            
            //OR elimination subproofs on p ∨ q
            6 SubProof(
                7 Assume(  p  ),
                8 (     F       )   by NegE(7, 2)
            ),
            9 SubProof(
                10 Assume(  q  ),
                11 (    F       )   by NegE(10, 3)
            ),
            
            //use OrE rule to tie together subproofs
            12 (        F       )   by OrE(5, 6 , 8)

            //want to reach a contradiction
        ),
        //will use NegI to conclude ¬(p ∨ q)
    )
```

All that remains is the use the `NegI` rule to finish subproof 4, as that subproof ended with a contradiction:

```text
(¬p ∧ ¬q) ⊢ (¬(p ∨ q))
    Proof(
        1 (     ¬p ∧ ¬q     )   by Premise,
        2 (     ¬p          )   by AndE1(1),
        3 (     ¬q          )   by AndE2(1),

        4 SubProof(
            5 Assume (  p ∨ q  ),
            
            //OR elimination subproofs on p ∨ q
            6 SubProof(
                7 Assume(  p  ),
                8 (     F       )   by NegE(7, 2)
            ),
            9 SubProof(
                10 Assume(  q  ),
                11 (    F       )   by NegE(10, 3)
            ),
            
            //use OrE rule to tie together subproofs
            12 (        F       )   by OrE(5, 6 , 8)

            //want to reach a contradiction
        ),
        //will use NegI to conclude ¬(p ∨ q)
        13 (        ¬(p ∨ q)    )   by NegI(4)
    )
```

## Example 2

When doing propositional logic translations, we learned that `p → q` is equivalent to its *contrapositive*, `¬q → ¬p`. We will prove one direction of this equivalence (to show two statements are *provably equivalent,* which we will see in section 4.8, we would need to prove both directions):

```text
(p → q) ⊢ (¬q → ¬p)
```

We notice that the top-level operator of our conclusion is an IMPLIES operator, so we know that we need to introduce an implies operator. We saw in the previous section that the blueprint for introducing an implies operator is with a subproof: assume the left side of the goal implies statement, and try to reach the right side of the goal implies statement by the end of the subproof.

We have this proof structure:

```text
(p → q) ⊢ (¬q → ¬p)
    Proof(
        1 (     p → q       )   by Premise,

        2 SubProof(
            3 Assume(  ¬q  ),

            //goal: reach ¬p
        ),
        //use →i to conclude ¬q → ¬p
    )
```

We see that our goal in the subproof is the show ¬p -- if we could do that, then we could tie together that subproof with the →i rule. Since our intermediate goal is to prove NOT (something), then we use our strategy for negation introduction as an inner subproof. We finish the proof as shown:

```text
(p → q) ⊢ (¬q → ¬p)
    Proof(
        1 (     p → q       )   by Premise,

        2 SubProof(
            3 Assume(  ¬q  ),

            //use ¬i strategy to prove ¬p
            4 SubProof(
                5 Assume(  p  ),
                6 (     q       )   by NegE(1, 5),
                7 (     F       )   by NegE(6, 3)
            ),
            8 (     ¬p          )   by NegI(4)

            //goal: reach ¬p
        ),
        //use →i to conclude ¬q → ¬p

        9 (     ¬q → ¬p         )   by ImplyI(2)
    )
```

## Example 3

Suppose we want to prove the sequent:

```text
(¬(¬p ∨ ¬q)) ⊢ (p ∧ q)
```

We see that there is nothing to extract from our premise, and that the top-level operator of the conclusion (`p ∧ q`) is an AND. We see that we will need to introduce an AND statement -- but the only way we can create `p ∧ q` is to separately prove both `p` and `q`. 

It is not immediately clear how to prove either `p` or `q`. We will discuss proof strategies in more detail in the next section, but `PbC` is a good fallback option if you don't have a clear path for how to prove something and some of the claims in the proof involve negation. Since we wish to prove `p`, then will will assume `¬p` in a subproof. If we can reach a contradiction, then we can use `PbC` to conclude `p`:

```text
(¬(¬p ∨ ¬q)) ⊢ (p ∧ q)
    Proof(
        1 (     ¬(¬p ∨ ¬q)      )       by Premise,

        2 SubProof(
            3 Assume(  ¬p  ),

            //goal: contradiction
        ),
        //use PbC to conclude p

        //similarly prove q

        //use AndI to conclude p ∧ q
    )
```

In subproof 2, we know we need to end with a contradiction. The only propositions we have to work with are `¬(¬p ∨ ¬q)` and `¬p`. But if we use `OrI1` with `¬p`, then we could have `¬p ∨ ¬q` -- and then we could claim a contradiction. We complete the proof as shown (using the same strategy to prove `q`):

```text
(¬(¬p ∨ ¬q)) ⊢ (p ∧ q)
    Proof(
        1 (     ¬(¬p ∨ ¬q)      )   by Premise,

        2 SubProof(
            3 Assume(  ¬p  ),
            4 (     ¬p ∨ ¬q     )   by OrI1(3),
            5 (     F           )   by NegE(4, 1) 

            //goal: contradiction
        ),
        //use PbC to conclude p

        6 (     p               )   by PbC(2),

        //similarly prove q
        7 SubProof(
            8 Assume(  ¬q  ),
            9 (     ¬p ∨ ¬q     )   by OrI2(8),
            10 (    F           )   by NegE(9, 1) 

            //goal: contradiction
        ),
        11 (        q           )   by PbC(7),

        //use AndI to conclude p ∧ q
        12 (        p ∧ q       )   by AndI(6, 11)
    )
```

## Law of the excluded middle

The *law of the excluded middle (LEM)* is famous consequence of `PbC`: from no starting premises at all, we can prove `p ∨ ¬ p` for any proposition we can imagine:

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

In other proofs involving negation and no clear path forward, it is sometimes useful to first derive LEM (this is possible since no premises are needed). If we have the claim `p ∨ ¬ p` in a proof, then we can use OR elimination where we separately assume `p` and then `¬ p` to try to reach the same conclusion. Here is one such example:

```text
(p → q) ⊢ (¬p ∨ q)
    Proof(
        1 (     p → q       )   by Premise,
        
        // start of previous p ∨ ¬ p proof
        2 SubProof(
            3 Assume(  ¬(p ∨ ¬p)  ),

            4 SubProof(
                5 Assume(  p  ),
                6 (     p ∨ ¬ p         )   by OrI1(5),
                7 (     F               )   by NegE(6, 3)
            ),
            8 (     ¬p                  )   by NotI(4),
            9 (     p ∨ ¬p              )   by OrI2(8),
            10 (    F                   )   By NegE(9, 3)
        ),
        11 (        p ∨ ¬p              )   By PbC(2),  // conclusion of p ∨ ¬ p proof

        12 SubProof(
            13 Assume(  p  ),
            14 (        q               )   By ImplyE(1, 13), 
            15 (        ¬p ∨ q          )   By OrI2(14)      
        ),
        16 SubProof(
            17 Assume(  ¬p  ),
            18 (        ¬p ∨ q          )   By OrI1(17)
        ),
        19 (        ¬p ∨ q              )   By OrE(11, 12, 16)
    )
 ```