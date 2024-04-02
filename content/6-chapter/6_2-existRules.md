---
title: "Rules with ∃"
pre: "6.2. "
weight: 71
date: 2018-08-24T10:53:26-05:00
---

In this section, we will see the two inference rules for the existential (∃) quantifier.

## Exists introduction

We can use the exists introduction rule, `ExistsI[T]`, when we have a proposition of the form `P(a)` for an arbitrary member `a` of a domain. Since we found one individual where a proposition held, then we can also say that there exists an individual for which the proposition is true. We can formalize the rule as follows:

```text
                   P(d)         where  d  is an individual of type T
ExistsI[T]: ---------------------
              ∃((x: T) => P(x))
```

Here is a simple example showing the syntax of the `ExistsI[T]` rule (where *Socrates* is a parameter of type `T` to our proof function):

```text
(   isHuman(Socrates)   ) ⊢ (   ∃ x isHuman(x)  )
    
    Proof(
        1 (     isHuman(Socrates)   )   by Premise,
        2 (     ∃((x: T) => P(x))   )   by ExistsI[T](1)
    )
```

When we use the `ExistsI[T]` rule to justify a claim like `∃((x: T) => P(x))`, we include the line number of where the proposition held for a particular individual. In the proof above, we claim `∃((x: T) => P(x))` with justification `ExistsI[T](1)` -- line 1 corresponds to `isHuman(Socrates)`, where our `∃ x isHuman(x)` proposition held for a particular individual. The full proof function, which shows how *Socrates* can be accepted as a parameter of type `T`, is here:

```java
@pure def ExistsExample[T](isHuman: T => B @pure, Socrates: T): Unit = {
    Deduce(
        //@formatter: off
        (   isHuman(Socrates)   ) ⊢ (   ∃ x isHuman(x)  )
    
        Proof(
            1 (     isHuman(Socrates)   )   by Premise,
            2 (     ∃((x: T) => P(x))   )   by ExistsI[T](1)
        )
        //@formatter: on
    )
}
```



Note that we can use the `ExistsI[T]` rule to introduce any variable, not just `x`. You can choose which variable to introduce based on the variables used in the conclusion. For example, the following proof is also valid:

```text
(   isHuman(Socrates)   ) ⊢ (   ∃ x isHuman(z)  )
    
    Proof(
        1 (     isHuman(Socrates)   )   by Premise,
        2 (     ∃((z: T) => P(z))   )   by ExistsI[T](1)
    )
```

## Exists elimination

Since the `ExistsI[T]`-rule constructs propositions that begin with `∃`, the `ExistsE[T]`-rule (exists elimination) disassembles propositions that begin with `∃`.

Here is a quick example (where our domain is living things):

- All humans are mortal
- Someone is human
- Therefore, someone is mortal

We don't know the name of the human, but it does not matter. Since ALL humans are mortal and SOMEONE is human, then our anonymous human must be mortal. The steps will go like this:

- Since "someone is human" and since we do not know his/her name, we will just make up our own name for them – "Jane". So, we assume that "Jane is human".

- We use the logic rules we already know to prove that "Jane is mortal".

- Therefore, SOMEONE is mortal and their name does not matter.

This approach is coded into the last logic law, `ExistsE[T]` (exists elimination).

Suppose we have a premise of the form `∃((x: T) => P(x))`. Since we do not know the name of the individual "hidden" behind the `∃(x: T)`, we make up a name for it, say `a`, and discuss what must follow from the assumption that `P(a)` holds true. Here is the formalization of the `ExistsE[T]` rule:

```text
                                        Let ((a: T) => SubProof(          // where  a  is a new, fresh name
                                             Assume( P(a) ),              // a  MUST NOT appear in  Q
                ∃((x: T) => P(x))            ...
                                             Q         
                                        )),             
ExistsE[T]: ----------------------------------------------------
                     Q
```

That is, if we can deduce `Q` from `P(a)`, and we do not mention `a` within `Q`, then it means `Q` can be deduced no matter what name the hidden individual has. So, `Q` follows from `∃((x: T) => P(x))`.

We can work the previous example, with `ExistsE[T]`:

```text
All humans are mortal
Someone is human
Therefore, someone is mortal
```

We make up the name, `jane`, for the human whose name we do not know:

```text
    (
        ∀((h: T) => (isHuman(h) → isMortal(h))),
        ∃((x: T) => isHuman(x))
    )
⊢
    (
        ∃((y: T) => (isMortal(y)))
    )
Proof(
    1 (     ∀((h: T) => (isHuman(h) → isMortal(h)))     )   by Premise,
    2 (     ∃((x: T) => isHuman(x))                     )   by Premise,

    3 Let ( (jane: T) => SubProof (
        4 Assume(   isHuman(jane)                       ),
        5 (         isHuman(jane) → isMortal(jane)      )   by AllE[T](1),
        6 (         isMortal(jane)                      )   by ImplyE(5, 4)
    )),
    7 (     ∃((y: T) => (isMortal(y)))                  )   by ExistsE[T](2, 3)
)
```

Line 3 proposes the name `jane` for our subproof and line 4 makes the assumption `isHuman(jane)` (based on the premise `∃((x: T) => isHuman(x))`). The subproof leads to Line 6, which says that someone is mortal. (We never learned the individual's name!) Since Line 6 does not explicitly mention the made-up name, `jane`, we use Line 7 to repeat Line 6 – without knowing the name of the individual "hiding" inside Line 2, we made a subproof that proves the result, anyway. This is how `ExistsE[T]` works.

Note that when we use the `ExistsE[T]` rule as a justification we include first the line number of the there exists statement that we processed (by naming the hidden individual) in the previous subproof, and then the line number of that subproof. In the example above, we say `ExistsE[T](2, 3)` because line 2 includes the there-exists statement we processed (`∃ ∃((x: T) => isHuman(x))`) in the previous subproof and line 3 is the subproof.

When using `ExistsE[T]`, the previous subproof must begin with introducing a name for a hidden individual in a there-exists statement and then immediately make an assumption that substitutes the name into the there exists statement. The last line in the subproof should contain NO mention of the chosen name. Whatever we claim on the last line in the subproof, we must claim EXACTLY the same thing immediately afterwards when we use the `ExistsE[T]` rule.

You are welcome to use any name for the hidden individual -- not just `jane` or `a`. The only restriction is that you cannot have used the name anywhere else in the proof.

## Examples

In this section, we will look at other proofs involving the existential quantifier.

### Example 1

Suppose we wish to prove the following sequent:

```text
(   ∃((x: T) => (Adult(x) ∨ Kid(x)))  ) ⊢ (   ∃((x: T) => Adult(x) )) ∨ ∃((x: T) => Kid(x)  )
```

We will begin as we did previously: by introducing an alias for our person that is either an adult or a kid (say, `alice`):

```text
(   ∃((x: T) => (Adult(x) ∨ Kid(x)))  ) ⊢ (   ∃((x: T) => Adult(x) )) ∨ ∃((x: T) => Kid(x)  )

Proof(
    1 (     ∃((x: T) => (Adult(x) ∨ Kid(x)) )       )   by Premise,

    2 Let ( (alice: T) => SubProof(
        3 Assume(   Adult(alice) ∨ Kid(alice)       ),
        
        //goal: get to our conclusion, ∃( (x: T) => Adult(x) )) ∨ ∃( (x: T) => Kid(x)
    )),

    //use ExistsE[T] to restate our conclusion, since we know SOME such person is either an adult or kid
)
```

To finish our proof, we can use OR elimination on `Adult(alice) ∨ Kid(alice)`, and then `ExistsE[T]` afterwards to restate our conclusion. Here is the completed proof:

```text
(   ∃((x: T) => (Adult(x) ∨ Kid(x)))  ) ⊢ (   ∃((x: T) => Adult(x) )) ∨ ∃((x: T) => Kid(x)  )

Proof(
    1 (     ∃((x: T) => (Adult(x) ∨ Kid(x)) )                   )   by Premise,

    2 Let ( (alice: T) => SubProof(
        3 Assume(   Adult(alice) ∨ Kid(alice)                   ),

        4 SubProof(
            5 Assume (  Adult(alice)                            ),
            6 (     ∃((x: T) => Adult(x)                        )   by ExistsI[T](5),
            7 (     ∃((x: T) => Adult(x) ∨ ∃((x: T) => Kid(x)   )   by OrI1(6)
        ),
        8 SubProof(
            9 Assume (  Kid(alice)                              ),
            10 (    ∃((x: T) => Kid(x)                          )   by ExistsI[T](9),
            11 (    ∃((x: T) => Adult(x) ∨ ∃((x: T) => Kid(x)   )   by OrI2(10)
        ),
        12 (    ∃((x: T) => Adult(x) ∨ ∃((x: T) => Kid(x)       )   by OrE(3, 4, 8)
        
        //goal: get to our conclusion, ∃( (x: T) => Adult(x) )) ∨ ∃( (x: T) => Kid(x)
    )),
    //use ExistsE[T] to restate our conclusion, since we know SOME such person is either an adult or kid

    13 (        ∃((x: T) => Adult(x) ∨ ∃((x: T) => Kid(x)       )   by ExistsE[T](1, 2)
)
```

### Example 2

Suppose we wish to prove the following (in the domain of living things):

- All bunnies are fluffy
- There is a fast bunny
- Therefore, there is a creature that is fast and fluffy

We can translate our premises and desired conclusion to predicate logic, and write the following sequent:

```text
    (
        ∀((x: T) => (Bunny(x) → Fluffy(x))),
        ∃((x: T) => (Fast(x) & Bunny(x))) 
    )
⊢
    (
        ∃((x: T) => (Fast(x) & Fluffy(x)))
    )
```

Since we are trying to prove a claim about some individual, it makes sense that we would start the process of an `ExistsE[T]` subproof where we introduce an alias (`thumper`) for the fast bunny. We will try to reach the conclusion by the end of that subproof. Here is the setup:

```text
    (
        ∀((x: T) => (Bunny(x) → Fluffy(x))),
        ∃((x: T) => (Fast(x) & Bunny(x))) 
    )
⊢
    (
        ∃((x: T) => (Fast(x) & Fluffy(x)))
    )
Proof(
    1 (     ∀((x: T) => (Bunny(x) → Fluffy(x)))     )   by Premise,
    2 (     ∃((x: T) => (Fast(x) & Bunny(x)))       )   by Premise,

    3 Let ( (thumper: T) => SubProof(
        4 Assume(   Fast(thumper) ∧ Bunny(thumper)  ),
        5 (         Fast(thumper)                   )   by AndE1(4),
        6 (         Bunny(thumper)                  )   by AndE2(4),

        //goal: ∃((x: T) => (Fast(x) & Fluffy(x)))
    )),

    //use ExistsE[T] to restate ∃((x: T) => (Fast(x) & Fluffy(x))), since we know there is SOME fast bunny
)
```

To finish our subproof, we see that we have a proposition about all creatures (`∀((x: T) => (Bunny(x) → Fluffy(x)))`) and that we are working with an individual creature (`thumper`). We can use `AllE[T]` to prove `Bunny(thumper) → Fluffy(thumper)`. After that, we have a few more manipulations using propositional logic rules, our `ExistsI[T]` rule to transition our claim from being about our alias `thumper` to being about an unnamed individual, and then our `ExistsE[T]` rule to pull our final claim out of the subproof. Here is the completed proof:

```text
    (
        ∀((x: T) => (Bunny(x) → Fluffy(x))),
        ∃((x: T) => (Fast(x) & Bunny(x))) 
    )
⊢
    (
        ∃((x: T) => (Fast(x) & Fluffy(x)))
    )
Proof(
    1 (     ∀((x: T) => (Bunny(x) → Fluffy(x)))         )   by Premise,
    2 (     ∃((x: T) => (Fast(x) & Bunny(x)))           )   by Premise,

    3 Let ( (thumper: T) => SubProof(
        4 Assume(   Fast(thumper) ∧ Bunny(thumper)      ),
        5 (         Fast(thumper)                       )   by AndE1(4),
        6 (         Bunny(thumper)                      )   by AndE2(4),
        7 (         Bunny(thumper) → Fluffy(thumper)    )   by AllE[T](1),
        8 (         Fluffy(thumper)                     )   by ImplyE(7, 6),
        9 (         Fast(thumper) ∧ Fluffy(thumper)     )   by AndI(5, 8),
        10 (        ∃((x: T) => (Fast(x) & Fluffy(x)))  )   by ExistsE[T](9)

        //goal: ∃((x: T) => (Fast(x) & Fluffy(x)))
    )),
    //use ExistsE[T] to restate ∃((x: T) => (Fast(x) & Fluffy(x))), since we know there is SOME fast bunny

    11 (    ∃((x: T) => (Fast(x) & Fluffy(x)))          )   by ExistsE[T](2, 3)
)
```