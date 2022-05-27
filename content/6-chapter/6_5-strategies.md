---
title: "Summary and Strategies"
pre: "6.5. "
weight: 74
date: 2018-08-24T10:53:26-05:00
---

## Reminder: propositional logic rules are still available

When doing proofs in predicate logic, remember that all the deduction rules from propositional logic are still available. You will often want to use the same strategies we saw there -- not introduction to prove a NOT, implies introduction to create an implies statement, OR elimination to process an OR statement, etc. 

However, keep in mind that propositional logic rules can only be used on statements without quantifiers as their top-level operator. For example, if we have the statement `∀ x (S(x) ∧ Pz(x))`, then we cannot use `∧e` -- the top-level operator is a universal quantifier, and the `∧` statement is "bound up" in that quantifier. We would only be able to use `∧e` after we had used `∀ e` to eliminate the quantifier.