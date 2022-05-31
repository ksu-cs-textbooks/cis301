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

We also see that the truth table for LEM is a tautology:

```text
      *
-------------
p | p ∨ ¬ p
-------------
T |   T F
F |   T T
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
⊢ (p → q) → ((¬p → q) → q)
```

We see that the top-level operator of what we are trying to prove is an implies operator. So, we begin our proof using the strategy for implies introduction:

```text
⊢ (p → q) → ((¬p → q) → q)
{
    1. {
        2. p → q                assume

        //goal: reach (¬p → q) → q
    }
    //use →i to conclude (p → q) → ((¬p → q) → q)
}
```

Inside subproof 1, we are trying to prove `(¬p → q) → q`. The top-level operator of that statement is an implies, so we nest another subproof with the goal of using implies introduction:

```text
⊢ (p → q) → ((¬p → q) → q)
{
    1. {
        2. p → q                assume

        3. {
            4. ¬p → q           assume

            //goal: reach q
        }

        //use →i to conclude (¬p → q) → q

        //goal: reach (¬p → q) → q
    }
    //use →i to conclude (p → q) → ((¬p → q) → q)
}
```

Now we must prove `q` in subproof 3. We have available propositions `p → q` and `¬p → q` -- we can see that if we had LEM (`p ∨ ¬p`) available, then we could use OR elimination to get our `q` in both cases. We insert the LEM proof into subproof 3:

```text
⊢ (p → q) → ((¬p → q) → q)
{
    1. {
        2. p → q                        assume

        3. {
            4. ¬p → q                   assume

            //Begin LEM proof, p ∨ ¬p
            5. {
                6. ¬ (p ∨ ¬ p)          assume
                7. {
                    8. p                assume
                    9. p ∨ ¬ p          ∨i1 8
                    10. ⊥               ¬e 9 6
                }
                11.  ¬ p                ¬i 7
                12.  p ∨ ¬ p            ∨i2 11
                13.  ⊥                  ¬e 12 6
            }
            14. p ∨ ¬ p                 pbc 5

            //End LEM proof for p ∨ ¬p

            //use OR elimination on p ∨ ¬p 

            //goal: reach q
        }

        //use →i to conclude (¬p → q) → q

        //goal: reach (¬p → q) → q
    }
    //use →i to conclude (p → q) → ((¬p → q) → q)
}
```

Finally, we do OR elimination with `p ∨ ¬p` and tie together the rest of the proof:

```text
⊢ (p → q) → ((¬p → q) → q)
{
    1. {
        2. p → q                        assume

        3. {
            4. ¬p → q                   assume

            //Begin LEM proof, p ∨ ¬p
            5. {
                6. ¬ (p ∨ ¬ p)          assume
                7. {
                    8. p                assume
                    9. p ∨ ¬ p          ∨i1 8
                    10. ⊥               ¬e 9 6
                }
                11.  ¬ p                ¬i 7
                12.  p ∨ ¬ p            ∨i2 11
                13.  ⊥                  ¬e 12 6
            }
            14. p ∨ ¬ p                 pbc 5

            //End LEM proof for p ∨ ¬p

            //use OR elimination on p ∨ ¬p, try to reach q
            15. {
                16. p               assume
                17. q               →e 2 16
            }
            18. {
                19. ¬ p             assume
                20. q               →e 4 19
            }
            21. q                   ∨e 14 15 18

            //goal: reach q
        }

        //use →i to conclude (¬p → q) → q
        22. (¬p → q) → q            →i 3

        //goal: reach (¬p → q) → q
    }
    //use →i to conclude (p → q) → ((¬p → q) → q)

    23. (p → q) → ((¬p → q) → q)    →i 1
}
```

If we complete a truth table for `(p → q) → ((¬p → q) → q)`, we also see that it is a tautology:

```text
              *
-------------------------------
p q | (p → q) → ((¬p → q) → q)
-------------------------------
T T |    T    T   F  T    T
T F |    F    T   F  T    F
F T |    T    T   T  T    T
F F |    T    T   T  F    T
------------------------------
Tautology
```