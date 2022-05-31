---
title: "Rules with ∃"
pre: "6.2. "
weight: 71
date: 2018-08-24T10:53:26-05:00
---

In this section, we will see the two inference rules for the existential (∃) quantifier.

## Exists introduction

We can use the exists introduction rule, `∃i`, when we have a proposition of the form `P(a)` for an arbitrary member `a` of a domain. Since we found one individual where a proposition held, then we can also say that there exists an individual for which the proposition is true. We can formalize the rule as follows:

```text
       P(d)         where  d  is an individual
∃i: -----------
      ∃ x P(x)
```

Here is a simple example showing the syntax of the `∃i` rule in Logika:

```text
isHuman(Socrates) ⊢  ∃ x isHuman(x)
{
    1. isHuman(Socrates)            premise
    2. ∃ x isHuman(x)               ∃i 1 Socrates
}
```

When we use the `∃i` rule to justify a claim like `∃ x P(x)`, we include the line number of where the proposition held for a particular individual, as well as the name of the individual. In the proof above, we claim `∃ x isHuman(x)` with justification `∃i 1 Socrates` -- line 1 corresponds to `isHuman(Socrates)`, where our `∃ x isHuman(x)` proposition held for a particular individual. The `Socrates` part of the justification is the name of the individual.

Note that we can use the `∃i` rule to introduce any variable, not just `x`. You can choose which variable to introduce based on the variables used in the conclusion. For example, the following proof is also valid:

```text
isHuman(Socrates) ⊢  ∃ z isHuman(z)
{
    1. isHuman(Socrates)            premise
    2. ∃ z isHuman(z)               ∃i 1 Socrates
}
```

## Exists elimination

Since the `∃i`-rule constructs propositions that begin with `∃`, the `∃e`-rule (exists elimination) disassembles propositions that begin with `∃`.

Here is a quick example (where our domain is living things):

- All humans are mortal
- Someone is human
- Therefore, someone is mortal

We don't know the name of the human, but it does not matter. Since ALL humans are mortal and SOMEONE is human, then our anonymous human must be mortal. The steps will go like this:

- Since "someone is human" and since we do not know his/her name, we will just make up our own name for them – "Jane". So, we assume that "Jane is human".

- We use the logic rules we already know to prove that "Jane is mortal".

- Therefore, SOMEONE is mortal and their name does not matter.

This approach is coded into the last logic law, `∃e` (exists elimination).

Suppose we have a premise of the form `∃ x P(x)`. Since we do not know the name of the individual "hidden" behind the `∃ x`, we make up a name for it, say `a`, and discuss what must follow from the assumption that `P(a)` holds true. Here is the formalization of the `∃e` rule:

```text
                  {a  P(a)   assume       // where  a  is a new, fresh name
      ∃ x P(x)      ...  Q         }      // a  MUST NOT appear in  Q
∃e: -----------------------------------
                     Q
```

That is, if we can deduce `Q` from `P(a)`, and we do not mention `a` within `Q`, then it means `Q` can be deduced no matter what name the hidden individual has. So, `Q` follows from `∃  P(x)`.

We can work the previous example, with `∃e`:

```text
All humans are mortal
Someone is human
Therefore, someone is mortal
```

We make up the name, `jane`, for the human whose name we do not know:

```text
∀ h(isHuman(h) → isMortal(h)), ∃ x isHuman(x) |- ∃ y isMortal(y)
{
    1. ∀ h(isHuman(h) → isMortal(h))        premise
    2. ∃ x isHuman(x)                       premise
    3. {
        4. jane isHuman(jane)               assume
        5. isHuman(jane) → isMortal(jane)   ∀e 1 jane
        6. isMortal(jane)                   →e 5 4
        7. ∃y isMortal(jane)                ∃i 6 ajane
    }
    8. ∃y isMortal(y)                       ∃e 2 3
}
```

Line 4 proposes the name `jane` and the assumption that `isHuman(jane)`. The subproof leads to Line 7, which says that someone is mortal. (We never learned the individual's name!) Since Line 7 does not explicitly mention the made-up name, `jane`, we use Line 8 to repeat Line 7 – without knowing the name of the individual "hiding" inside Line 2, we made a subproof that proves the result, anyway. This is how `∃e` works.

Note that when we us the `∃e` rule as a justification we include first the line number of the there-exists statement that we processed (by naming the hidden individual) in the prrevious subproof, and then the line number of that subproof. In the example above, we say `∃e 2 3` because line 2 includes the there-exists statement we processed (`∃ x isHuman(x)`) in the previous subproof and line 3 is the subproof.

When using `∃e`, the previous subproof must begin with introducting a name for a hidden individual in a there-exists statement and then immediately substituting that name into the there-exists statement. The justification on the first line is always `assume`. The last line in the subproof should contain NO mention of the chosen name. Whatever we claim on the last line in the subproof, we must claim EXACTLY the same thing immediately afterwards when we use the `∃e` rule.

You are welcome to use any name for the hidden individual -- not just `jane` or `a`. The only restriction is that you cannot have used the name anywhere else in the proof.

## Examples

In this section, we will look at several proofs involving the existential quantifier.

### Example 1

Suppose we wish to prove the following sequent:

```text
∃ x (Human(x)) ⊢ ∃ y (Human(y))
```

Following the same approach in the `∃e` example above, we know that there is SOME human. Let's introduce the alias `bob` for that human:

```text
∃ x (Human(x)) ⊢ ∃ y (Human(y))
{
    1. ∃ x (Human(x))               premise
    2. {
        3. bob Human(bob)           assume

        //goal: get to our conclusion, ∃ y (Human(y))
    }
    //use ∃e to restate our conclusion, since we know SOME such human exists
}
```

Since we have `Human(bob)` in our subproof, we can use `∃i` in our subproof to instead say that there exists some human. We will introduce the `y` variable, since that's what we want in our conclusion:

```text
∃ x (Human(x)) ⊢ ∃ y (Human(y))
{
    1. ∃ x (Human(x))               premise
    2. {
        3. bob Human(bob)           assume
        4. ∃ y (Human(y))           ∃i 3 bob

        //goal: get to our conclusion, ∃ y (Human(y))
    }
    //use ∃e to restate our conclusion, since we know SOME such human exists
}
```

All that remains is to use `∃e` to restate our conclusion after the subproof. Since we knew someone was a human, and since we reached a claim that didn't use our alias, then we can restate the result outside the scope of the subproof:

```text
∃ x (Human(x)) ⊢ ∃ y (Human(y))
{
    1. ∃ x (Human(x))               premise
    2. {
        3. bob Human(bob)           assume
        4. ∃ y (Human(y))           ∃i 3 bob

        //goal: get to our conclusion, ∃ y (Human(y))
    }
    //use ∃e to restate our conclusion, since we know SOME such human exists
    5. ∃ y (Human(y))               ∃e 1 2
}
```

### Example 2

Suppose we wish to prove the following sequent:

```text
∃ x (Adult(x) ∨ Kid(x)) ⊢ (∃ x Adult(x)) ∨ (∃ x Kid(x))
```

We will begin as we did previously: by introducing an alias for our person that is eitehr an adult or a kid (say, `alice`):

```text
∃ x (Adult(x) ∨ Kid(x)) ⊢ (∃ x Adult(x)) ∨ (∃ x Kid(x))
{
    1. ∃ x (Adult(x) ∨ Kid(x))              premise
    2. {
        3. alice Adult(alice) ∨ Kid(alice)  assume

        //goal: get to our conclusion, (∃ x Adult(x)) ∨ (∃ x Kid(x))
    }
    //use ∃e to restate our conclusion, since we know SOME such person is either an adult or kid
}
```

To finish our proof, we can use OR elimination on `Adult(alice) ∨ Kid(alice)`, and then `∃e` afterwards to restate our conclusion. Here is the completed proof:

```text
∃ x (Adult(x) ∨ Kid(x)) ⊢ (∃ x Adult(x)) ∨ (∃ x Kid(x))
{
    1. ∃ x (Adult(x) ∨ Kid(x))                  premise
    2. {
        3. alice Adult(alice) ∨ Kid(alice)      assume
        4. {
            5. Adult(alice)                     assume
            6. ∃ x Adult(x)                     ∃i 5 alice
            7. (∃ x Adult(x)) ∨ (∃ x Kid(x))    ∨i1 6
        }
        8. {
            9. Kid(alice)                       assume
            10. ∃ x Kid(x)                      ∃i  9 alice
            11. (∃ x Adult(x)) ∨ (∃ x Kid(x))   ∨i2 10
        }
        12. (∃ x Adult(x)) ∨ (∃ x Kid(x))       ∨e 3 4 8

        //goal: get to our conclusion, (∃ x Adult(x)) ∨ (∃ x Kid(x))
    }
    //use ∃e to restate our conclusion, since we know SOME such person is either an adult or kid

    13. (∃ x Adult(x)) ∨ (∃ x Kid(x))           ∃e 1 2
}
```

### Example 3

Suppose we wish to prove the following (in the domain of living things):

- All bunnies are fluffy
- There is a fast bunny
- Therefore, there is a creature that is fast and fluffy

We can translate our premises and desired conclusion to predicate logic, and write the following sequent:

```text
∀ x (Bunny(x) → Fluffy(x)), ∃ x (Fast(x) ∧ Bunny(x)) ⊢ ∃ x (Fast(x) ∧ Fluffy(x))
```

Since we are trying to prove a claim about some individual, it makes sense that we would start the process of an `∃e` subproof where we introduce an alias (`thumper`) for the fast bunny. We will try to reach the conclusion by the end of that subproof. Here is the setup:

```text
∀ x (Bunny(x) → Fluffy(x)), ∃ x (Fast(x) ∧ Bunny(x)) ⊢ ∃ x (Fast(x) ∧ Fluffy(x))
{
    1. ∀ x (Bunny(x) → Fluffy(x))                   premise
    2. ∃ x (Fast(x) ∧ Bunny(x))                     premise
    3. {
        4. thumper Fast(thumper) ∧ Bunny(thumper)   assume
        5. Fast(thumper)                            ∧e1 4
        6. Bunny(thumper)                           ∧e2 4

        //goal: ∃ x (Fast(x) ∧ Fluffy(x))
    }

    //use ∃e to restate ∃ x (Fast(x) ∧ Fluffy(x)), since we know there is SOME fast bunny
}
```

To finish our subproof, we see that we have a proposition about all creatures (`∀ x (Bunny(x) → Fluffy(x))`) and that we are working with an individual creature (`thumper`). We can use `∀e` to prove `Bunny(thumper) → Fluffy(thumper)`. After that, we have a few more manipulations using propositional logic rules, our `∃i` rule to transition our claim from being about our alias `thumper` to being about an unnamed individual, and then our `∃e` rule to pull our final claim out of the subproof. Here is the completed proof:

```text
∀ x (Bunny(x) → Fluffy(x)), ∃ x (Fast(x) ∧ Bunny(x)) ⊢ ∃ x (Fast(x) ∧ Fluffy(x))
{
    1. ∀ x (Bunny(x) → Fluffy(x))                   premise
    2. ∃ x (Fast(x) ∧ Bunny(x))                     premise
    3. {
        4. thumper Fast(thumper) ∧ Bunny(thumper)   assume
        5. Fast(thumper)                            ∧e1 4
        6. Bunny(thumper)                           ∧e2 4
        7. Bunny(thumper) → Fluffy(thumper)         ∀e 1 thumper
        8. Fluffy(thumper)                          →e 7 6
        9. Fast(thumper) ∧ Fluffy(thumper)          ∧i 5 8
        10. ∃ x (Fast(x) ∧ Fluffy(x))               ∃i 9 thumper

        //goal: ∃ x (Fast(x) ∧ Fluffy(x))
    }
    //use ∃e to restate ∃ x (Fast(x) ∧ Fluffy(x)), since we know there is SOME fast bunny

    11. ∃ x (Fast(x) ∧ Fluffy(x))                   ∃e 2 3
}
```