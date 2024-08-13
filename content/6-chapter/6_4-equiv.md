---
title: "Equivalence"
pre: "6.4. "
weight: 74
date: 2018-08-24T10:53:26-05:00
---

In Chapter 5, we saw DeMorgan's laws for quantifiers -- that if we have some domain, and if `P(x)` is a predicate for individuals in that domain, then the following statements are equivalent:

- `¬(∃ x P(x))` is equivalent to `∀ x ¬P(x)`
- `¬(∀ x P(x))` is equivalent to `∃ x ¬P(x)`

The process of proving that two predicate logic statements are equivalent is the same as it was in propositional logic -- we must prove the second proposition using the first as a premise, and we must prove the first given the second as a premise. 

## Example - how to prove equivalence

For example, to prove that `¬(∃ x P(x))` is equivalent to `∀ x ¬P(x)`, we must prove the sequents:

```text
    (
        ¬(∃((x: T) => P(x)))
    )
⊢
    (
        ∀((x: T) => ¬P(x))
    )
```

and

```text
    (
        ∀((x: T) => ¬P(x))
    )
⊢
    (
        ¬(∃((x: T) => P(x)))
    )
```

We prove both directions below:

```text
    (
        ¬(∃((x: T) => P(x)))
    )
⊢
    (
        ∀((x: T) => ¬P(x))
    )
Proof(
    1 (     ¬(∃((x: T) => P(x)))        ) by Premise,

    2 Let ( (a: T) => SubProof(

        3 SubProof(
            4 Assume (  P(a)  ),
            5 (     ∃((x: T) => P(x))   ) by ExistsI[T](3),
            6 (     F                   ) by NegE(4, 1)
        ),
        7 (     ¬P(a)                   ) by NegI(3)

    )),
    8 (  ∀((x: T) => ¬P(x))             ) by AllI[T](2)
)

```

And:

```text
    (
        ∀((x: T) => ¬P(x))
    )
⊢
    (
        ¬(∃((x: T) => P(x)))
    )
Proof(
    1 (     ∀((x: T) => ¬P(x))          )   by Premise,

    2 SubProof(
        3 Assume (  ∃((x: T) => P(x))   ),

        4 Let ( (a: T) => SubProof(
            5 Assume (  P(a)  ),
            6 (     ¬P(x)               )   by AllE[T](1),
            7 (     F                   )   by NegE(5, 6),
        )),
        8 (     F                       )   By ExistsE[T](3, 4)
    
    ),
    9 (     ¬(∃((x: T) => P(x)))        )   by NegI(2)
)
```

## More extensive list of equivalences

Here is a more extensive list of equivalences in predicate logic. The remaining proofs are left as exercises for the reader:

- `¬(∃ x P(x))` is equivalent to `∀ x ¬P(x)`
- `¬(∀ x P(x))` is equivalent to `∃ x ¬P(x)`
-  `∀ x (P(x) → ¬Q(x))` is equivalent to `¬(∃ x P(x) ∧ Q(x))`
- `∀ x ∀ y P(x, y)` is equivalent to `∀ y ∀ x P(x, y)`
- `∃ x ∃ y P(x, y)` is equivalent to `∃ y ∃ x P(x, y)`
- `Q ∧ (∀ x P(x))` is equivalent to `∀ x (Q ∧ P(x))` (where `x` does not appear in `Q`)
- `Q v (∀ x P(x))` is equivalent to `∀ x (Q V P(x))` (where `x` does not appear in `Q`)
- `Q ∧ (∃ x P(x))` is equivalent to `∃ x (Q ∧ P(x))` (where `x` does not appear in `Q`)
- `Q V (∃ x P(x))` is equivalent to `∃ x (Q V P(x))` (where `x` does not appear in `Q`)