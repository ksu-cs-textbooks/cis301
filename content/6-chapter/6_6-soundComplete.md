---
title: "Soundness and Completeness"
pre: "6.6. "
weight: 76
date: 2018-08-24T10:53:26-05:00
---

## Soundness and completeness definitions

We now revisit the notions of *soundness* and *completeness*. We recall from propositional logic that a proof system is *sound* if everything that is provable is actually true. A proof system is *complete* if everything that is true can be proved.

## Interpretations

When we write statements in logic, we use predicates and function symbols (e.g., `∀ i (i * 2) > i`). An interpretation gives the meaning of:

- The underlying domain – what set of elements it names

- Each function symbol – what answers it computes from its parameters from the domain

- Each predicate – which combinations of arguments from the domain lead to true answers and false answers

### Interpretation example - integers

Here is an example. Say we have the function symbols: `+`, `-`, `*`, and `/`, and the predicate symbols: `>` and `=`. What do these names and symbols mean? We must interpret them.

The standard interpretation of arithmetic is that:
-  `int` names the set of all integers

- `+`, `-`, `*`, and `/` name the integer addition, subtraction, multiplication, and division functions (that take two integers as "parameters" and "return" an integer result)

- `=` and `>` name integer equality comparison and integer less-than comparison predicates (that take two integers as "parameters" and "return" the boolean result of the comparison)

 With this interpretation of arithmetic, we can interpret statements. For example,`∀ i (i * 2) > i` interprets to `false`, as when `i` is negative, then `i * 2 < i`. Similarly, `∃ j (j * j) = j` interprets to `true`, as `1 * 1 = 1`. 

Now, given the function names `+`, `-`, `*`, `/`, and the predicates, `=`, `>`, we can choose to interpret them in another way. For example, we might interpret the underlying domain as just the *nonnegative* integers. We can interpret `+`, `*`, `/`, `>`, `=` as the usual operations on ints, but we must give a different meaning to `-`. We might define `m - n == 0`, whenever `n > m`.

Yet another interpretation is to say that the domain is just `{0, 1}`; the functions are the usual arithmetic operations on 0 and 1 under the modulus of 2. For example, we would define `1 + 1 == 0` because `(1 + 1) mod 2 == 0`. We would define `1 > 0` as `true` and any other evaluation of `<` as false.

These three examples show that the symbols in a logic can be interpreted in *multiple different ways*. 

### Interpretation example - predicates

Here is a second example. There are no functions, and the predicates are *IsMortal(_)*, *IsLeftHanded(_)* and *IsMarriedTo(_,_)*. An interpretation might make all (living) members of the human race as the domain; make *IsMortal(h)* defined as `true` for every human `h`; make `IsLeftHanded(j)` defined as true for exactly those humans, `j`, who are left handed; and set `IsMarriedTo(x, y)` as `true` for all pairs of humans (`x`, `y`) who have their marriage document in hand.

We can ask whether a proposition is true within ONE specific interpretation, and we can ask whether a proposition is true within ALL possible interpretations. This leads to the notions of soundness and completeness for predicate logic. 

## Valid sequents in predicate logic

>A sequent, `P_1, P_2, ..., P_n ⊢ Q` is *valid* in an interpretation, `I`, provided that when all of `P_1`, `P_2`, ..., `P_n` are true in interpretation `I`, then so is `Q`. The sequent is valid exactly when it is valid in ALL possible interpretations.

## Soundness and completeness in predicate logic

We can then define soundness and completeness for predicate logic:

> *soundness*: When we use the deduction rules to prove that `P_1, P_2, ..., P_n ⊢ Q`, then the sequent is valid (in all possible interpretations)

> *completeness:* When `P_1, P_2, ..., P_n ⊢ Q` is valid (in all possible interpretations), then we can use the deduction rules to prove the sequent.

Note that, if `P_1, P_2, ..., P_n ⊢ Q` is valid in just ONE specific interpretation, then we are not guaranteed that our rules will prove it. This is a famous trouble spot: For centuries, mathematicians were searching for a set of deduction rules that could be used to build logic proofs of all the true propositions of arithmetic, that is, the language of `int`, `+`, `-`, `*`, `/`, `>`, and `=`. No appropriate rule set was devised.

In the early 20th century, Kurt Gödel showed that it is impossible to formulate a sound set of rules customized for arithmetic that will prove exactly the true facts of arithmetic. Gödel showed this by formulating true propositions in arithmetic notation that talked about the computational power of the proof rules themselves, making it impossible for the proof rules to reason completely about themselves. The form of proposition he coded in logic+arithmetic stated "I cannot be proved". If this proposition is false, it means the proposition can be proved. But this would make the rule set unsound, because it proved a false claim. The only possibility is that the proposition is true (and it cannot be proved). Hence, the proof rules remain sound but are incomplete.

Gödel’s construction, called *diagonalization*, opened the door to the modern theory of computer science, called *computability theory*, where techniques from logic are used to analyze computer programs. You can study these topics more in CIS 570 and CIS 575.

Computability theory includes the notion of *decidability* -- a problem that is decidable CAN be solved by a computer, and one that is undecidable cannot. A famous example of an undecidable problem is the *Halting problem*: given an arbitrary computer program and program input, can we write a checker program that will tell if the input program will necessarily terminate (halt) on its input? The answer is NO - the checker wouldn't work on itself.