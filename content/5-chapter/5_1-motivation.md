---
title: "Motivation"
pre: "5.1. "
weight: 60
date: 2018-08-24T10:53:26-05:00
---

In this chapter, we will learn to further decompose statements in terms of their verbs (called *predicates*) and their nouns (called *individuals*). This leads to *predicate logic* (also called *first-order logic*).

As a motivation of why we want more expressive power, suppose we wanted to translate the following statements to propositional logic:

```text
All humans are mortal.
Socrates is a human.
Socrates is mortal.
```

Unfortunately, each statement would be a propositional atom:

```text
p: All humans are mortal.
q: Socrates is a human.
r: Socrates is mortal.
```

But what if we wanted to prove that given the premises: "All humans are mortal" and "Socrates is a human", that the conclusion "Socrates is mortal" naturally followed? This logical argument makes sense -- Socrates is a human, and all such individuals are supposed to be mortal, so it should follow that Socrates is mortal. If we tried to write such a proof in propositional logic, though, we would have the sequent:

```text
p, q ⊢ r
```

...and we clearly don't have enough information to complete this proof.

We need a richer language, which we will get with predicate logic. 