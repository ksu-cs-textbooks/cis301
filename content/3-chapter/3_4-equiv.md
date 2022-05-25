---
title: "Equivalent Translations"
pre: "3.4. "
weight: 43
date: 2018-08-24T10:53:26-05:00
---

As we saw in [section 2.4]({{<ref "2-chapter/2_4-logicalEquiv.md" >}})), two logical statements are said to be *logically equivalent* if and only if they have the same truth value for every truth assignment.

We can extend this idea to our propositional logic translations -- two (English) statements are said to be *equivalent* iff they have the same underlying meaning, and iff their translations to propositional logic are logically equivalent.

## Common equivalences, revisited

We previously identified the following common logical equivalences:

- Double negative: `¬ ¬ p` and `p`
- Contrapositive: `p → q` and `¬ q → ¬ p`
- Expressing an implies using an OR: `p → q` and `¬ p ∨ q`
- One of DeMorgan's laws: `¬ (p ∧ q)` and `( ¬ p ∨ ¬ q)`
- Another of DeMorgan's laws: `¬ (p ∨ q)` and `( ¬ p ∧ ¬ q)`

## Equivalence example 1

Suppose we have the following propositional atoms:

```text
p: I get cold
q: It is summer
```

Consider the following three statements:

- *I get cold except possibly if it is summer.*
- *If it's not summer, then I get cold.*
- *I get cold or it is summer.*

We translate each sentence to propositional logic:

- *I get cold except possibly if it is summer.*
    - `p →  ¬q`
    - Meaning: I promise that if I get cold, then it must not be summer...because I am always cold when it's not summer.

- *If it's not summer, then I get cold.*
    - ` ¬q → p`
    - Meaning: I promise that anytime it isn't summer, then I will get cold.

- *I get cold or it is summer.*
   - `p V q`
   - Meaning: I'm either cold or it's summer...because my being cold is true every time it isn't summer.

As we can see, each of these statements is expressing the same idea.

## Equivalence example 2

Suppose we have the following propositional atoms:

```text
p: I eat chips
q: I eat fries
```

Consider the following two statements:

- *I don't eat both chips and fries.*
- *I don't eat chips and/or I don't eat fries.*

We translate each sentence to propositional logic:

- *I don't eat both chips and fries.*
    - ` ¬(p ∧ q)`

- *I don't eat chips and/or I don't eat fries.*
    - ` ¬p V  ¬q`

These statements are clearly expressing the same idea -- if it's not the case that I eat both, then it's also true that there is at least one of the foods that I don't eat. This is an application of one of DeMorgan's laws: that `¬ (p ∧ q)` is equivalent to `( ¬ p ∨ ¬ q)`.

If we were to create truth tables for both ` ¬(p ∧ q)` and ` ¬p V  ¬q`, we would see that they are *logically equivalent* (that the same truth assignments make each statement true).

## Equivalence example 3

Using the same propositional atoms as example 2, we consider two more statements:

- *I don't eat chips or fries.*
- *I don't eat chips and I don't eat fries.*

We translate each sentence to propositional logic:

- *I don't eat chips or fries.*
    - ` ¬(p V q)`

- *I don't eat chips and I don't eat fries.*
    - ` ¬p ∧  ¬q`

These statements are clearly expressing the same idea -- I have two foods (chips and fries), and I don't eat either one. This demonstrates another of DeMorgan's laws: that `¬ (p ∨ q)` is equivalent to `( ¬ p ∧ ¬ q)`. If we were to create truth tables for each statement, we would see that they are logically equivalent as well.