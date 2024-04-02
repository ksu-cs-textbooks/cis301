+++
title = "Predicate Logic Proofs"
date = 2018-08-24T10:53:05-05:00
weight = 7
archetype = "chapter"
ordinal = "6"
pre = "<b>6. </b>"
+++

Now that we have seen how to translate statements to predicate logic, we will learn new deduction rules for working with universal and existential quantifiers. We will be able to add those rules to our propositional logic deduction rules and show that a set of premises proves a conclusion in predicate logic. Predicate logic is also referred to as *first order logic*. 

As with propositional logic, we can use the Logika tool to help check the correctness of our new deduction rules. However, these new rules also exist outside of Logika, and we could express the same proofs with our rules in a different environment or on paper -- the concepts are the same.

## Logika predicate logic proof syntax

We will use the following format in Logika to start a natural deduction proof for predicate logic. Each proof will be saved in a new file with a `.sc` (Scala) extension:

```text
// #Sireum #Logika
//@Logika: --manual --background type

import org.sireum._
import org.sireum.justification._
import org.sireum.justification.natded.prop._
import org.sireum.justification.natded.pred._

@pure def ProofName[T](pred1: T => B @pure, pred2: T => B @pure, ..., indiv1: T, indiv2: T, ...): Unit = {
    Deduce(
        //@formatter:off
        (comma-separated list of premises with variable1, variable2, ...)  ⊢  (conclusion)
            Proof(
                //the actual proof steps go here
            )
        //@formatter:on
    )
}
```

Here, `T` is the type of elements in our domain. Usually, we will just use `T` to denote a generic type (much like generics in Java and C#), but occasionally we will use specific types like `Z` (which means "integer"). Next, `pred1`, `pred2`, etc. are the predicates for our proofs. The `T => B` means that they take an element in our domain as a parameter (which has type `T`) and return a boolean (which has type `B`). Finally, `indiv1`, `indiv2`, etc. are specific individuals within our domain, each of which have type `T`.

A proof function like the example above can have as many or few predicates and individuals as are needed for the proof.

As was the case with propositional logic, the examples in this chapter will omit the imports, proof function definition, deduce call, and formatter changes are omitted here for readability. We will start each example with the sequent followed by the Proof call.


## For all statements in Logika

The syntax of a statement like `∀ x P(x)` in Logika is a little different, since we must specify that each `x` is an element in our domain. When we are using Logika to do a predicate logic proof, we express `∀ x P(x)` as:

```text
∀ ((x: T) => P(x))
```

The above statement is saying, "for all x that are of the type T, P(x) is true".

We can alternatively use curly braces instead of standard parentheses to surround our predicate:

```text
∀ {(x: T) => P(x)}
```

Such statements can be a pain to type out manually, so there is a way to insert them using a template. If you right-click where you wish to write a predicate logic statement, you can select *Slang->Insert Template->Forall*. This will insert the statement:

```text
∀((ID: TYPE) => CLAIM)
```

You can type your variable name (often `x`, `y`, etc.) in place of `ID`, the domain type (usually `T`) in place of `TYPE`, and your claim (with predicates and propositional logic operators) in place of `CLAIM`.

Finally, you can type the keyboard shortcut *Ctrl+Shift+\, A* to insert a for all statement.


## There exists statements in Logika

Statements with existential quantifiers like `∃ x P(x)` must also be treated differently. When we are using Logika to do a predicate logic proof, we express `∃ x P(x)` as:

```text
∃ ((x: T) => P(x))
```

The above statement is saying, "there exists an x with type T where P(x) is true".

We can alternatively use curly braces instead of standard parentheses to surround our predicate:

```text
∃ {(x: T) => P(x)}
```

Such statements can be a pain to type out manually, so there is a way to insert them using a template. If you right-click where you wish to write a predicate logic statement, you can select *Slang->Insert Template->Exists*. This will insert the statement:

```text
∃((ID: TYPE) => CLAIM)
```

You can type your variable name (often `x`, `y`, etc.) in place of `ID`, the domain type (usually `T`) in place of `TYPE`, and your claim (with predicates and propositional logic operators) in place of `CLAIM`.

Finally, you can type the keyboard shortcut *Ctrl+Shift+\, E* to insert a there exists statement.
