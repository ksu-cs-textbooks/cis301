---
title: "Introduction"
pre: "4.1. "
weight: 50
date: 2018-08-24T10:53:26-05:00
---

While we can use truth tables to check whether a set of premises entail a conclusion, this requires testing all possible truth assignments -- of which there are exponentially many. In this chapter, we will learn the process of *natural deduction* in propositional logic. This will allow us to start with a set of known facts (*premises*) and apply a series of rules to see if we can reach some goal *conclusion*. Essentially, we will be able to see whether a given conclusion necessarily follows from a set of premises.

We will use the Logika tool to check whether our proofs correctly follow our deduction rules. HOWEVER, these proofs can and do exist outside of Logika. Different settings use slightly different syntaxes for the deduction rules, but the rules and proof system are the same. We will merely use Logika to help check our work.

## Sequents, premises, and conclusions

A *sequent* is a mathematical term for an assertion or an argument. We use the notation:

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
p → q , ¬q  ⊢  ¬p
```

To check if this sequent is valid, we must find all truth assignments for which both premises are true, and then ensure that those truth assignments also make the conclusion true.

### Sequent validity in Logika using truth tables

We can use a different kind of truth table to prove the validity of a sequent in Logika:

```text
         *     *     *
---------------------------
p q # (p →: q, ¬q) ⊢ ¬p
---------------------------
T T #    T     F     F
T F #    F     T     F
F T #    T     F     T
F F #    T     T     T
---------------------------
Valid [F F]
```

Notice that instead of putting just one logical formula, we put the entire sequent -- the premises are in a comma-separated list inside parentheses, then the turnstile operator (which we type using the keys `|-` in Logika), and then the conclusion. We mark the top-level operator of each premise and conclusion.

Examining each row in the above truth table, we see that only the truth assignment [F F] makes both premises (`p → q` and ` ¬q`) true. We look right to see that the same truth assignment also makes the conclusion (` ¬p`) true, which means that the sequent is valid.

## Proving sequents using natural deduction

Now we will turn to the next section of the course -- using *natural deduction* to similarly prove the validity of sequents. Instead of filling out truth tables (which becomes cumbersome very quickly), we will apply a series of *deduction rules* to allow us to conclude new claims from our premises. In turn, we can use our deduction rules on these new claims to conclude more and more, until (hopefully) we are able to claim our conclusion. If we can do that, then our sequent was valid.

## Logika natural deduction proof syntax

We will use the following format in Logika to start a natural deduction proof for propositional logic. Each proof will be saved in a new file with a `.sc` (Scala) extension:

```text
// #Sireum #Logika
//@Logika: --manual --background type

import org.sireum._
import org.sireum.justification._
import org.sireum.justification.natded.prop._

@pure def ProofName(variable1: B, variable2: B, ...): Unit = {
    Deduce(
        (comma-separated list of premises with variable1, variable2, ...)  ⊢  (conclusion)
            Proof(
                //the actual proof steps go here
            )
    )
}
```

Once we are inside the `Proof` element (where the above example says "the actual proof steps go here"), we complete a numbered series of steps. Each step includes a claim and corresponding justification, like this:

```text
// #Sireum #Logika
//@Logika: --manual --background type

import org.sireum._
import org.sireum.justification._
import org.sireum.justification.natded.prop._

@pure def ProofName(variable1: B, variable2: B, ...): Unit = {
    Deduce(
        (comma-separated list of premises with variable1, variable2, ...)  ⊢  (conclusion)
            Proof(
                1 (     claim_a         )   by Justification_a,
                2 (     claim_b         )   by Justification_b,
                ...
                736 (   conclusion      )   by Justification_conc
            )
    )
}
```

Each claim is given a number, and these numbers are generally in order. However, the only rule is that claim numbers be unique (they may be out of order and/or non-consecutive). Once we have justified a claim in a proof, we will refer to it as a *fact*.

We will see more details of Logika proof syntax as we progress through chapter 4.

## Premise justification

The most basic justification for a claim in a proof is "premise". This justification is used when you pull in a premise from the sequent and introduce it into your proof. All, some or none of the premises can be introduced at any time in any order. Please note that only one premise may be entered per claim.

For example, we might bring in the premises from our sequent like this (the imports, proof function definition, deduce call, and formatter changes are omitted here for readability):

```text
(p, q, ¬r)  ⊢  (p ∧ q)
    Proof(
        1 (     p               )   by Premise,
        2 (     q               )   by Premise,
        3 (     ¬r              )   by Premise,
        ...
    )
```

We could also bring in the same premise multiple times, if we wanted. We could also use non-sequential line numbers, as long as each line number was unique:

```text
(p, q, ¬r)  ⊢  (p ∧ q)
    Proof(
        7 (     p           )   by Premise,
        10 (    q           )   by Premise,
        2 (     ¬r          )   by Premise,
        8 (     p           )   by Premise,
        ...
    )
```

We could only bring in some portion of our premises, if we wanted:

```text
(p, q, ¬r)  ⊢  (p ∧ q)
    Proof(
        1 (     p           )   by Premise,
        ...
    )
```

But we can only list one premise in each claim. For example, the following is not allowed:

```text
(p, q, ¬r)  ⊢  (p ∧ q)
    Proof(
        1 (     p, q, ¬r           )   by Premise,      //NO! Only one premise per line.
        ...
    )
```

## Deduction rules

The logical operators (AND, OR, NOT, IMPLIES) are a kind of language for building propositions from basic, primitive propositional atoms. For this reason, we must have laws for constructing propositions and for disassembling them. These laws are called *inference* rules or *deduction* rules. A *natural deduction system* is a set of inference rules, such that for each logical operator, there is a rule for constructing a proposition with that operator (this is called an *introduction rule*) and there is a rule for disassembling a proposition with that operator (this is called an *elimination rule*).

For the sections that follow, we will see the introduction and elimination rules for each logical operator. We will then learn how to use these deduction rules to write a formal proof showing that a sequent is valid. 