---
title: "Negation Rules"
pre: "4.5. "
weight: 54
date: 2018-08-24T10:53:26-05:00
---

In this section, we will see the deduction rules for the NOT operator. In this section, we will introduce the contradiction symbol (`⊥`, or `_|_` in ASCII). This symbol is also referred to as the *bottom operator*. We will be able to claim that a *contradiction* occurs when, for some proposition `P`, we have proved both the facts `P` and `¬ P`. This indicates that we are in an impossible situation, and often means that we have made a bad previous assumption.

## Not elimination

The not elimination rule allows you to claim that you have reached a contradiction. We can formalize the rule like this:

```text
        P   ¬ P
¬ e : -----------
          ⊥
```

Here is a simple example of a proof that shows the syntax of the `¬ e` rule:

```text
q, ¬ q ⊢ ⊥
{
    1. q                premise
    2. ¬ q              premise
    3. ⊥                ¬ e 1 2
}
```

We use the `¬ e` rule when we have facts for both `P` and `¬ P` for some proposition `P`. When we use the justification, we first list the line number of the claim for `P` (line 1, in our case) and then the line number of the claim for `¬ P` (line 2, in our case).

Sometimes, the proposition `P` itself has a NOT operator. Consider this example:

```text
¬ q, ¬ ¬ q ⊢ ⊥
{
    1. ¬ q              premise
    2. ¬ ¬ q            premise
    3. ⊥                ¬ e 1 2
}
```

Here, our proposition `P` is the claim `¬ q`, and our proposition that is of the form `¬ P` is the claim `¬ ¬ q`.

## Not introduction

The not introduction rule allows us to introduce a NOT operation. If assuming some proposition `P` leads to a contradiction, then we must have made a bad assumption -- `P` must NOT be true after all. We can then introduce the fact `¬ P`. We can formalize the not introduction rule like this:

```text
       { P assume
         ... ⊥  }
¬ i:   --------------
           ¬ P
```

Here is a simple example of a proof that shows the syntax of the `¬ i` rule:

```text
p,  q → ¬ p  ⊢  ¬ q
{
    1. p            premise
    2. q → ¬ p      premise
    3. {
        4. q        assume
        5. ¬ p      →e 2 4
        6. ⊥        ¬e 1 5
    }
    7. ¬ q          ¬i 3
}
```

Note that the not introduction rule involves a subproof -- if we wish to prove `¬ P` for some proposition `P`, then we start a subproof where we assume `P`. If we are able to reach a contradiciton on the last line of that subproof, then we can use the `¬ i` rule after the subproof ends to claim that our assumption was bad and that it is actually `¬ P` that is true. When we use `¬i` as a justification, we list the line number corresponding to this subproof.

## Bottom elimination

There is a special law for reasoning forwards from an impossible situation — the ⊥e law — which says, in the case of a contradiction, everything becomes a fact. (That is, "if False is a fact, so is everything else ¬".) This rule is called "bottom elimination", and is written as `⊥e`. Here is a formalization of the rule:

```text
         ⊥
⊥e :  ------  for any proposition, Q, at all
         Q
```

Here is a simple example of a proof that shows the syntax of the `⊥e` rule:

```text
p, ¬ p  ⊢ q
{
    1. p            premise
    2. ¬ p          premise
    3. ⊥            ¬ e 1 2
    4. q            ⊥e 3
}
```

Note that when we use `⊥e` as the justification, we list the line number of where we reached a contradiction (`⊥`).

The bottom elimination rule works well with case analysis, where we discover that one case is impossible. Here is a classic example:

```text
p ∨ q, ¬ p ⊢ q
{
    1. p ∨ q        premise
    2. ¬ p          premise
    3. {
        4. p        assume
        5. ⊥        ¬e 4 2
        6. q        ⊥e 5
    }
    7. {
        8. q        assume
    }
    9. q            ∨e 1 3 7
}
```

Considering the premise, `p ∨ q`, we analyze our two cases by starting OR elimination. The first case, where `p` holds true, is impossible, because it causes a contradiction. The `⊥e`-rule lets us gracefully prove `q` in this "impossible case". (You can read lines 4-6 as saying, "in the case when `p` might hold true, there is a contradiction, and in such an impossible situation, we can deduce whatever we like, so we deduce `q` to finish this impossible case".)

The second case, that `q` holds true, is the only realistic case, and it immediately yields the conclusion. The proof finishes the two-case analysis with the `∨e` rule.

## Proof by contradiction

The proof by contraction rule, `pbc`, says that when assuming `¬ P` leads to a contradiction for some proposition `P`, then we made a bad assumption and thus `P` must be true. It is very similar to the `¬ i` rule, except `pbc` has us assuming `¬ P` and eventually concluding `P`, while the `¬ i` rule has us assuming `P` and eventually concluding `¬ P`.

Here is a formalization of `pbc`:

```text
        { ¬ P assume
          ... ⊥   }
pbc:   ---------------
             P
```

And here is an example that demonstrates the syntax of the rule:

```text
¬ ¬ p ⊢ p
{
    1. ¬ ¬ p        premise
    2. {
        3. ¬ p      assume
        4. ⊥        ¬e 3 1
    }
    5. p            pbc 2
}
```

When we use the `pbc` rule as a justification for a claim `P`, we cite the line number of the subproof where we assumed `¬ P` and ended in a contradiction.

## Example 1

Suppose we want to prove the following sequent:

```text
¬p ∧ ¬q |- ¬(p ∨ q)
```

We start by listing our premise, and extracting the two sides of the AND statement:

```text
¬p ∧ ¬q ⊢ ¬(p ∨ q)
{
    1. ¬p ∧ ¬q          premise
    2. ¬p               ∧e1 1
    3. ¬q               ∧e2 1
    ...
}
```

Next, we see that our conclusion has the form NOT (something), so this tells us that we will need to introduce a NOT (using the `¬ i` rule). In fact, ANY time we wish to prove a proposition of the form NOT (something), we will always use the `¬ i` rule. (We will discuss proof strategies in detail in the next section.) Since we want to prove `¬ (p ∨ q)`, then we open a subproof where we assume `p ∨ q`. If we can end that subproof with a contradiction, then we can use `¬ i` afterwards to conclude `¬(p ∨ q)`.

We know that we want this proof structure:

```text
¬p ∧ ¬q ⊢ ¬(p ∨ q)
{
    1. ¬p ∧ ¬q          premise
    2. ¬p               ∧e1 1
    3. ¬q               ∧e2 1
    4. {
        5. p ∨ q        assume

        //want to reach a contradiction
    }
    //will use ¬ i to conclude ¬(p ∨ q)
}
```

We know we must reach a contradiction in our subproof. We see that we have claims `¬p`, `¬q`, and `p ∨ q `. Since at least one of `p ∨ q ` is true, and since either one would yield a contradiction with one of `¬p` or `¬q`, then we start on OR elimination:

```text
¬p ∧ ¬q ⊢ ¬(p ∨ q)
{
    1. ¬p ∧ ¬q          premise
    2. ¬p               ∧e1 1
    3. ¬q               ∧e2 1
    4. {
        5. p ∨ q        assume

        //OR elimination subproofs on p ∨ q
        6. {
            7. p        assume
            8. ⊥        ¬e 7 2
        }
        9. {
            10. q       assume
            11. ⊥       ¬e 10 3
        }

        //use ∨e rule to tie together subproofs

        //want to reach a contradiction
    }
    //will use ¬ i to conclude ¬(p ∨ q)
}
```

We see that both OR elimination subproofs ended with a contradiction (`⊥`). Just like any other use of `∨e`, we restate that common conclusion after the two subproofs. We knew at least one of `p` or `q` were true, and both ended in a contradiction -- so the contradiction holds no matter what:

```text
¬p ∧ ¬q ⊢ ¬(p ∨ q)
{
    1. ¬p ∧ ¬q          premise
    2. ¬p               ∧e1 1
    3. ¬q               ∧e2 1
    4. {
        5. p ∨ q        assume

        //OR elimination subproofs on p ∨ q
        6. {
            7. p        assume
            8. ⊥        ¬e 7 2
        }
        9. {
            10. q       assume
            11. ⊥       ¬e 10 3
        }

        //use ∨e rule to tie together subproofs
        12. ⊥           ∨e 5 6 9

        //want to reach a contradiction
    }
    //will use ¬ i to conclude ¬(p ∨ q)
}
```

All that remains is the use the `¬ i` rule to finish subproof 4, as that subproof ended with a contradiction:

```text
¬p ∧ ¬q ⊢ ¬(p ∨ q)
{
    1. ¬p ∧ ¬q          premise
    2. ¬p               ∧e1 1
    3. ¬q               ∧e2 1
    4. {
        5. p ∨ q        assume

        //OR elimination subproofs on p ∨ q
        6. {
            7. p        assume
            8. ⊥        ¬e 7 2
        }
        9. {
            10. q       assume
            11. ⊥       ¬e 10 3
        }

        //use ∨e rule to tie together subproofs
        12. ⊥           ∨e 5 6 9

        //want to reach a contradiction
    }
    //will use ¬ i to conclude ¬(p ∨ q)

    13. ¬(p ∨ q)        ¬ i 4
}
```

## Example 2

When doing propositional logic translations, we learned that `p → q` is equivalent to its *contrapositive*, `¬q → ¬p`. We will prove one direction of this equivalence (to show two statements are *provably equivalent,* which we will see in section 4.8, we would need to prove both directions):

```text
p → q ⊢ ¬q → ¬p
```

We notice that the top-level operator of our conclusion is an IMPLIES operator, so we know that we need to introduce an implies operator. We saw in the previous section that the blueprint for introducing an implies operator is with a subproof: assume the left side of the goal implies statement, and try to reach the right side of the goal implies statement by the end of the subproof.

We have this proof structure:

```text
p → q ⊢ ¬q → ¬p
{
    1. p → q                premise
    2. {
        3. ¬q               assume

        //goal: reach ¬p
    }
    //use →i to conclude ¬q → ¬p
}
```

We see that our goal in the subproof is the show ¬p -- if we could do that, then we could tie together that subproof with the →i rule. Since our intermediate goal is to prove NOT (something), then we use our strategy for not introduction as an inner subproof. We finish the proof as shown:

```text
p → q ⊢ ¬q → ¬p
{
    1. p → q                premise
    2. {
        3. ¬q               assume

        //use ¬i strategy to prove ¬p
        4. {
            5. p            assume
            6. q            →e 1 5
            7. ⊥            ¬e 6 3
        }
        8. ¬p               ¬i 4

        //goal: reach ¬p
    }
    //use →i to conclude ¬q → ¬p

    9. ¬q → ¬p              →i 2
}
```

## Example 3

Suppose we want to prove the sequent:

```text
¬(¬p ∨ ¬q) ⊢ p ∧ q
```

We see that there is nothing to extract from our premise, and that the top-level operator of the conclusion (`p ∧ q`) is an AND. We see that we will need to introduce an AND statement -- but the only way we can create `p ∧ q` is to separately prove both `p` and `q`. 

It is not immediately clear how to prove either `p` or `q`. We will discuss proof strategies in more detail in the next section, but `pbc` is a good fallback option if you don't have a clear path for how to prove something and some of the claims in the proof involve negation. Since we wish to prove `p`, then will will assume `¬p` in a subproof. If we can reach a contradiction, then we can use `pbc` to conclude `p`:

```text
¬(¬p ∨ ¬q) ⊢ p ∧ q
{
    1. ¬(¬p ∨ ¬q)           premise
    2. {
        3. ¬p               assume

        //goal: contradiction
    }
    //use pbc to conclude p

    //similarly prove q

    //use ∧i to conclude p ∧ q
}
```

In subproof 2, we know we need to end with a contradiction. The only propositions we have to work with are `¬(¬p ∨ ¬q)` and `¬p`. But if we use `∨i1` with `¬p`, then we could have `¬p ∨ ¬q` -- and then we could claim a contradiction ¬ We complete the proof as shown (using the same strategy to prove `q`):

```text
¬(¬p ∨ ¬q) ⊢ p ∧ q
{
    1. ¬(¬p ∨ ¬q)           premise
    2. {
        3. ¬p               assume
        4. ¬p ∨ ¬q          ∨i1 3

        //goal: contradiction
        5. ⊥                ¬e 4 1
    }
    //use pbc to conclude p

    6. p                    pbc 2

    //similarly prove q
    7. {
        8. ¬q               assume
        9. ¬p ∨ ¬q          ∨i2 8
        10. ⊥               ¬e 9 1
    }
    11. q                   pbc 7

    //use ∧i to conclude p ∧ q
    12. p ∧ q               ∧i 6 11
}
```

## Law of the excluded middle

The *law of the excluded middle (LEM)* is famous consequence of `pbc`: from no starting premises at all, we can prove `p ∨ ¬ p` for any proposition we can imagine:

```text
⊢ p ∨ ¬ p
{
    1. {
        2. ¬ (p ∨ ¬ p)      assume
        3. {
            4. p            assume
            5. p ∨ ¬ p      ∨i1 4
            6. ⊥            ¬e 5 2
        }
        7.  ¬ p             ¬i 3
        8.  p ∨ ¬ p         ∨i2 7
        9.  ⊥               ¬e 8 2
    }
    10. p ∨ ¬ p             pbc 1
}
```

In other proofs involving negation and no clear path forward, it is sometimes useful to first derive LEM (this is possible since no premises are needed). If we have the claim `p ∨ ¬ p` in a proof, then we can use OR elimination where we separately assume `p` and then `¬ p` to try to reach the same conclusion. Here is one such example:

```text
p → q ⊢ ¬ p ∨ q
{
    1. p → q                premise

    2. {                             // start of previous p ∨ ¬ p proof
        3. ¬ (p ∨ ¬ p)      assume
        4. {
            5. p            assume
            6. p ∨ ¬ p      ∨i1 5
            7. ⊥            ¬e 6 3
        }
        8.  ¬ p             ¬i 4
        9.  p ∨ ¬ p         ∨i2 8
        10. ⊥               ¬e 9 3
    }
    11. p ∨ ¬ p             pbc 2    // conclusion of p ∨ ¬ p proof

    12. {
        13. p               assume
        14. q               →e 1 13
        15. ¬ p ∨ q         ∨i2 14
    }
    16. {
        17. ¬ p             assume
        18. ¬ p ∨ q         ∨i1 17
    }
    19. ¬ p ∨ q             ∨e 11 12 16
 }
 ```