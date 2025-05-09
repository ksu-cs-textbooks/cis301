---
title: "Equivalence"
pre: "4.8. "
weight: 57
date: 2018-08-24T10:53:26-05:00
---

In this section, we will revisit the notion of equivalence. In chapter 2, we saw how we could use truth tables to show that two logical formulae are equivalent. Here, we will see that we can also show they are equivalent using our natural deduction proof rules.

## Semantic equivalence

We saw in section 2.4 that two (or more) logical statements `S1` and `S2` were said to be *semantically equivalent* if and only if:

```text
S1 ⊨ S2
```

and 

```text
S2 ⊨ S1
```

As a reminder, the `S1 ⊨ S2` means *`S1` semantically entails `S2`*, which means that every truth assignment that satisfies `S1` also satisfies `S2`. 

Semantic equivalence between `S1` and `S2` means that each proposition semantically entails the other -- that `S1` and `S2` have the same truth value for every truth assignment; i.e., their truth tables evaluate exactly the same. 

### Showing semantic equivalence with two truth tables

For example, if we wished to show that the propositions `p → ¬q` and `¬ (p ∧ q)` were semantically equivalent, then we could create truth tables for each proposition:

```text
        *
--------------
p q # p →: ¬q
--------------
T T #   F  F
T F #   T  T
F T #   T  F
F F #   T  T
--------------

Contingent
T: [T F] [F T] [F F]
F: [T T]
```

```text
      *
----------------
p q # ¬(p ∧ q)
----------------
T T # F   T
T F # T   F
F T # T   F
F F # T   F
----------------

Contingent
T: [T F] [F T] [F F]
F: [T T]
```

We see that the same set of truth assignments, `[T F] [F T] [F F]`, satisfies both `p →: ¬q` and `¬(p ∧ q)`.

### Showing semantic equivalence with one truth table

To show that propositions `S1` and `S2` are semantically equivalent, we need to show that if `S1` is true, then so is `S2`, and that if `S2` is true, then so is `S1`. Instead of comparing the truth tables of both `S1` and `S2`, we could instead express our requirements as a bi-implication: `S1 ↔ S2`. To express a bi-implication operator, we can use a conjunction of two implications: `(S1 → S2) ∧ (S2 → S1)`. If this conjunction is a tautology, then we know that if one proposition is true, then the other one is too -- that `S1` and `S2` are semantically equivalent.

Below, we show that `p →: ¬q` and `¬(p ∧ q)` are semantically equivalent using one truth table:

```text
                              *
-------------------------------------------------------
p q # ((p →: ¬q) →: ¬(p ∧ q)) ∧ (¬(p ∧ q) →: (p →: ¬q))
-------------------------------------------------------
T T #     F  F    T F   T     T  F   T    T     F  F 
T F #     T  T    T T   F     T  T   F    T     T  T
F T #     T  F    T T   F     T  T   F    T     T  F
F F #     T  T    T T   F     T  T   F    T     T  T
-------------------------------------------------------
Tautology
```

## Provable equivalence

Two propositional logic statements `S1` and `S2` are *provably equivalent* if and only if we can prove both of the following sequents:

```text
(S1) ⊢ (S2)
```

and

```text
(S2) ⊢ (S1)
```

We can also write:

```text
(S2) ⟛ (S1)
```

For example, suppose we wish to show that the propositions `p → ¬q` and `¬(p ∧ q)` are provably equivalent. We must prove the following sequents:

```text
(p → ¬q) ⊢ (¬(p ∧ q))
```

and

```text
(¬(p ∧ q)) ⊢ (p → ¬q)
```

We complete both proofs below:

```text
(p → ¬q) ⊢ (¬(p ∧ q))
    Proof(
        1 (     p → ¬q      )   by Premise,

        2 SubProof(
            3 Assume(  p ∧ q  ),
            4 (     p       )   by AndE1(3),
            5 (     q       )   by AndE2(3),
            6 (     ¬q      )   by ImplyE(1, 4),
            7 (     F       )   by NegE(5, 6)
        ),
        8 (     ¬(p ∧ q)    )   by NegI(2)
    )
```

```text
(¬(p ∧ q)) ⊢ (p → ¬q)
    Proof(
        1 (     ¬(p ∧ q)            )   by Premise,

        2 SubProof(
            3 Assume (  p  ),

            4 SubProof(
                5 Assume(  q  ),
                6 (     p ∧ q       )   by AndI(3, 5),
                7 (     F           )   by NegE(6, 1)
            ),
            8 (     ¬q              )   by NegI(4)
        ),
        9 (     p → ¬q              )   by ImplyI(2)
    )
```


