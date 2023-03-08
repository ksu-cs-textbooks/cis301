---
title: "Equivalence"
pre: "6.4. "
weight: 73
date: 2018-08-24T10:53:26-05:00
---

In Chapter 5, we saw DeMorgan's laws for quantifiers -- that if we have some domain, and if `P(x)` is a predicate for individuals in that domain, then the following statements are equivalent:

- `¬(∃ x P(x))` is equivalent to `∀ x ¬P(x)`
- `¬(∀ x P(x))` is equivalent to `∃ x ¬P(x)`

The process of proving that two predicate logic statements are equivalent is the same as it was in propositional logic -- we must prove the second proposition using the first as a premise, and we must prove the first given the second as a premise. 

## Example - how to prove equivalence

For example, to prove that `¬(∃ x P(x))` is equivalent to `∀ x ¬P(x)`, we must prove the sequents:

```text
¬(∃ x P(x)) ⊢ ∀ x ¬P(x)
```

and

```text
∀ x ¬P(x) ⊢ ¬(∃ x P(x))
```

We prove both directions below:

```text
¬(∃ x P(x)) ⊢ ∀ x ¬P(x)
{
    1. ¬(∃ x P(x))              premise
    2. {
        3. a
        4. {
            5. P(a)             assume
            6. ∃ x P(x)         ∃i 5 a
            7. ⊥                ¬e 6 1
        }
        8. ¬P(a)                ¬i 4
    }
    9. ∀ x ¬P(x)                ∀i 2
}
```

and

```text
∀ x ¬P(x) ⊢ ¬(∃ x P(x))
{
    1. ∀ x ¬P(x)                premise
    2. {
        3. ∃ x P(x)             assume
        4. {
            5. a P(a)           assume
            6. ¬P(a)            ∀i 1 a
            7. ⊥                ¬e 5 6
        }
        8. ⊥                    ∃e 3 4
    }
    9. ¬(∃ x P(x))              ¬i 2
}
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