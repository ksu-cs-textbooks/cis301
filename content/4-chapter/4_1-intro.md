---
title: "Introduction"
pre: "4.1. "
weight: 50
date: 2018-08-24T10:53:26-05:00
---

In this chapter, we will learn the process of *natural deduction* in propositional logic. This will allow us to start with a set of known facts (*premises*) and apply a series of rules to see if we can reach some goal *conclusion*. Essentially, we will be able to see whether a given conclusion necessarily follows from a set of premises.

## Sequents, premises, and conclusions

A *sequent* is a mathematical term for an assertion. We use the notation:

```text
p0, p1, ..., pm ⊢ c
```

The *p0, p1, ..., pm* are called *premises* and *c* is called the *conclusion*. The `⊢` is called the *turnstile operator*, and we read it as "prove". The full sequent is read as:

```text
Statements p0, p1, ..., pm PROVE c
```

A sequent is saying that if we accept statements *p0, p1, ..., pm* as facts, then we guarantee that *c* is a fact as well.

For example, in the sequent:

```text
p → q , ¬ q  ⊢  ¬ p
```

The premises are: `p → q` and ` ¬q`, and the conclusion is ` ¬p`. 

(Shortcut: we can use `|-` in place of the `⊢` turnstile operator.)


## Sequent validity

A sequent is said to be *valid* if, for every truth assignment which make the premises true, then the conclusion is also true.

For example, consider the following sequent:

```text
p → q , ¬ q  ⊢  ¬ p
```

To check if this sequent is valid, we must find all truth assignments for which both premises are true, and then ensure that those truth assignments also make the conclusion true.

Here is a (non-Logika syntax) type of truth table that combines all three statements:

```text
p q | (p → q) | ( ¬q) |  ¬p
---------------------------
T T |    T     |  F   | F
T F |    F     |  T   | F
F T |    T     |  F   | T
F F |    T     |  T   | T
```

Examining each row in the above truth table, we see that only the truth assignment [F F] makes both premises (`p → q` and ` ¬q`) true. We look right to see that the same truth assignment also makes the conclusion (` ¬p`) true, which means that the sequent is valid.

## Logika proof syntax

Each Logika proof should be written in a separate file with a .logika extension. Sequents in Logika have the following form:

```text
< 0 or more premises, separated by commas > ⊢ < 1 conclusion >
```

Proofs in Logika are structured in two columns, with claims on the left and their supporting justification on the right:

```text
premises ⊢ conclusion
{
    1. claim_a          justification_a
    2. claim_b          justification_b
       ...              ...
    736. conclusion     justification_ef
}
```

Each claim is given a number, and these numbers are generally in order. However, the only rule is that claim numbers be unique (they may be out of order and/or non-consecutive). Once we have justified a claim in a proof, we will refer to it as a *fact*.

We will see more details of Logika proof syntax as we progress through chapter 4.

## Premise justification

The most basic justification for a claim in a proof is "premise". This justification is used when you pull in a premise from the sequent and introduce it into your proof. All, some or none of the premises can be introduced at any time in any order. Please note that only one premise may be entered per claim.

For example, we might bring in the premises from our sequent like this:

```text
p, q,  ¬r |- p ∧ q
{
    1. p            premise
    2. q            premise
    3.  ¬r           premise
    ...
}
```

We could also bring in the same premise multiple times, if we wanted. We could also use non-sequential line numbers, as long as each line number was unique:

```text
p, q,  ¬r |- p ∧ q
{
    7. p            premise
    10. q           premise
    2.  ¬r           premise
    8. p            premise
    ...
}
```

We could only bring in some portion of our premises, if we wanted:

```text
p, q,  ¬r |- p ∧ q
{
    1. p            premise
    ...
}
```

But we can only list one premise in each claim. For example, the following is not allowed:

```text
p, q,  ¬r |- p ∧ q
{
    //THIS IS WRONG ¬

    1. p, q,  ¬r         premise
    ...
}
```

## Deduction rules

The logical operators (AND, OR, NOT, IMPLIES) are a kind of language for building statements from basic, primitive propositions. For this reason, we must have laws for constructing statements and for disassembling them. These laws are called *inference* rules or *deduction* rules. A *natural deduction system* is a set of inference rules, such that for each logical operator, there is a rule for constructing a statement with that operator (this is called an *introduction rule*) and there is a rule for disassembling a statement with that operator (this is called an *elimination rule*).

For the sections that follow, we will see the introduction and elimination rules for each logical operator. We will then learn how to use these deduction rules to write a formal proof showing that a sequent is valid. 