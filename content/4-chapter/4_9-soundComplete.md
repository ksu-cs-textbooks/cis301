---
title: "Soundness and Completeness"
pre: "4.9. "
weight: 58
date: 2018-08-24T10:53:26-05:00
---

Section 4.8 showed us that we can prove two statements are *semantically equivalent* with truth tables and *provably equivalent* with deduction proofs. Does it matter which approach we use? Will there ever be a time when two statements are semantically equivalent but not provably equivalent, or vice versa? Will there ever be a time when a set of premises semantically entails a conclusion, but that the premises do not prove (using our deduction proofs) the conclusion, or vice versa?

These questions lead us to the notions of *soundness* and *completeness*. Formal treatment of both concepts is beyond the scope of this course, but we will introduce both definitions and a rough idea of the proofs of soundness and completeness in propositional logic.

## Soundness

A proof system is *sound* if everything that is provable is actually true. Propositional logic is sound if when we use deduction rules to prove that `P1, P2, ..., Pn ⊢ C` (that a set of premises proves a conclusion) then we can also use a truth table to show that `P1, P2, ..., Pn ⊨ C` (that a set of premises semantically entails a conclusion).

**Propositional logic is, in fact, sound.** 

To get an idea of the proof, consider the `∧e1` deduction rule. It allows us to directly prove:

```text
P ∧ Q ⊢ P
```

I.e., if we have `P ∧ Q` as a premise or as a claim in part of a proof, then we can use `∧e1` to conclude `P`. We must also show that:

```text
P ∧ Q ⊨ P
```

I.e., that any time `P ∧ Q` is true in a truth table, then `P` is also true. And of course, we can examine the truth table for `P ∧ Q`, and see that it is only true in the cases that `P` is also true.

Consider the `∧i` deduction ule next. It allows us to directly prove:

```text
P, Q ⊢ P ∧ Q
```

I.e., if we have both `P` and `Q` as premises or claims in part of a proof, then we can use `∧i` to conclude `P ∧ Q`. We must also show that:

```text
P, Q ⊢ P ⊨ Q
```

I.e., that any time both `P` and `Q` are true in a truth table, then `P ∧ Q` is also true. And of course, we can examine the truth table for `P ∧ Q` and see that whenever `P` and `Q` are true, then `P ∧ Q` is also true.

To complete the soundness proof, we would need to examine the rest of our deduction rules in a similar process. We would then use an approach called *mathematical induction* (which we will see for other applications in Chapter 7) to extend the idea to a proof that applies multiple deduction rules in a row.

## Completeness

A proof system is *complete* if everything that is true can be proved. Propositional logic is complete if when we can use a truth table to show that `P1, P2, ..., Pn ⊨ C`, then we can also use deduction rules to prove that `P1, P2, ..., Pn ⊢ C`.

**Propositional logic is also complete.**  

We assume that `P1, P2, ..., Pn ⊨ C`, and we consider the truth table for `(P1 ∧ P2 ∧ ... ∧ Pn) → C` (since that will be a tautology whenever `P1, P2, ..., Pn ⊨ C`). In order to show propositional logic is complete, we must show that we can use our deduction rules to prove `P1, P2, ..., Pn ⊢ C`.

The idea is to use LEM for each propositional atom `A` to obtain `A ∨ ¬A` (corresponding to the truth assignments in the `(P1 ∧ P2 ∧ ... ∧ Pn) → C` truth table). We then use OR elimination on each combination of truth assignments, with separate cases for each logical operator being used.
