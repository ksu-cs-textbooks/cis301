---
title: "Logical Equivalence"
weight: 33
pre: "2.4. "
---

Two (or more) logical statements are said to be *logically equivalent* IFF (if and only if, ↔) they have the same truth value for every truth assignment; i.e., their truth tables evaluate exactly the same. (We sometimes refer to this as *semantic equivalence*.)

An example of logically equivalent statements are `q ∧ p` and `p ∧ (q ∧ p)`:

```text
         *
--------------
p q | (p ∧ q)
--------------
T T |  T T T
T F |  T F F
F T |  F F T
F F |  F F F
---------------
Contingent
- T : [T T]
- F : [F F] [F T] [T F]
```

```text
         *
-------------------
p q |  p ∧ (q ∧ p)
-------------------
T T |  T T  T T T
T F |  T F  F F T
F T |  F F  T F F
F F |  F F  F F F
--------------------
Contingent
- T : [T T]
- F : [F F] [F T] [T F]
```

In these examples, notice that exactly the same set of truth assignments makes both statements true, and that exactly the same set of truth assignments makes both statements false.

Finding equivalent logical statements of fewer gates (states) is important to several fields. In computer science, fewer states can lead to less memory, fewer operations and smaller programs. In computer engineering, fewer gates means fewer circuits less power and less heat.

## Common equivalences

We can similarly use truth tables to show the following common logical equivalences:

- Double negative: `¬ ¬ p` and `p`
- Contrapositive: `p → q` and `¬ q → ¬ p`
- Expressing an implies using an OR: `p → q` and `¬ p ∨ q`
- One of DeMorgan's laws: `¬ (p ∧ q)` and `( ¬ p ∨ ¬ q)`
- Another of DeMorgan's laws: `¬ (p ∨ q)` and `( ¬ p ∧ ¬ q)`

## Expressing additional operators

The bi-implication (`↔`) and exclusive or (`⊕`) operators are not directly used in this course. However, we can simulate both operators using a combination of `¬`, `∧`, `∨`, and `→`:

- `p ↔ q`, which means "p if and only if q", can be expressed as `(p → q) ∧ (q → p)`
- `p ⊕ q`, which means "p exclusive or q", can be expressed as `(p ∨ q) ∧ ¬(p ∧ q)`