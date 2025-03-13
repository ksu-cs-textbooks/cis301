---
title: "Rules with ∀"
pre: "6.1. "
weight: 71
date: 2018-08-24T10:53:26-05:00
---

Just as with our propositional logic operators, there will be two inference rules for each of our predicate logic quanitifiers -- an "introduction rule and an "elimination" rule.

In this section, we will see the two inference rules for the universal (∀) quantifier.

## For all elimination

For all elimination allows us to take a claim that uses a universal quantifier -- a statement about ALL individuals in a domain -- and make the same statement about a specific individual in the domain. After all, if the statement is true for ALL individuals, then it follows that it should be true for a particular individual. We can formalize the rule as follows:

```text
           ∀ ((x: T) => P(x))
AllE[T]:  ---------------------
                   P(v)     where v is a particular individual in the domain (i.e, v has type T)
```

Here is a simple example showing the syntax of the `AllE[T]` rule. It shows that given the premises: *All humans are mortal* and *Socrates is a human*, that we can prove that *Socrates is mortal*:

```text
    (   ∀ ((x: T) => (isHuman(x) → isMortal(x))),  
        isHuman(Socrates)    
    ) 
⊢ 
    (   
        isMortal(Socrates) 
    )
Proof(
    1 (     ∀ ((x: T) => (isHuman(x) → isMortal(x)))        )   by Premise,
    2 (     isHuman(Socrates)                               )   by Premise,
    3 (     isHuman(Socrates) → isMortal(Socrates)          )   by AllE[T](1),
    4 (     isMortal(Socrates)                              )   by ImplyE(3, 2)
)
```

We can read the justification `AllE[T](1)` as: "for all elimination of the for all statement on line 1.

While our `AllE[T]` justification does not mention the particular individual that was plugged in to the for all statement (*Socrates*, in this case), it is required that whatever individual we plug in has already been show to be of type `T`. This is done by accepting the individual as a parameter of type `T` to the proof function. The full proof function would look like this:

```text
@pure def socMortal[T](isHuman: T => B @pure, isMortal: T => B @pure, Socrates: T): Unit = {
    Deduce (
        (   
            ∀ ((x: T) => (isHuman(x) → isMortal(x))),  
            isHuman(Socrates)    
        ) 
        ⊢ 
        (   
            isMortal(Socrates) 
        )
        Proof(
            1 (     ∀ ((x: T) => (isHuman(x) → isMortal(x)))      )   by Premise,
            2 (     isHuman(Socrates)                             )   by Premise,
            3 (     isHuman(Socrates) → isMortal(Socrates)        )   by AllE[T](1),
            4 (     isMortal(Socrates)                            )   by ImplyE(3, 2)
        )
    )
}
```

In similar examples of `AllE[T]`, it is assumed that the named individual was accepted as a parameter of type `T` to the proof function.

## For all introduction

If we can show that a property of the form `P(a)` holds for an arbitrary member `a` of a domain, then we can use for all introduction to conclude that the property must hold for ALL individuals in the domain -- i.e., that `∀ x P(x)` (which we write in Logika as `∀ ((x: T) => P(x))`). We can formalize the rule as follows:

```text
            Let (   (a: T) => SubProof(
                ... 
                P(a)  
            )),
AllI[T] : -------------------------------
                 ∀ ((x: T) => P(x)) 

```

Here is a simple example showing the syntax of the `AllI[T]` rule: "Everyone is healthy; everyone is happy. Therefore, everyone is both healthy and happy.":

```text
    (   
        ∀((x: T) => isHealthy(x)), ∀((y: T) => isHappy(y))
    )  
⊢  
    (
        ∀((z: T) => isHealthy(z) ∧ isHappy(z))
    )
Proof (
    1 (     ∀((x: T) => isHealthy(x))               )   by Premise,
    2 (     ∀((y: T) => isHappy(y))                 )   by Premise,
    
    3 Let ((a: T) => SubProof(
        4 (     isHealthy(a)                        )   by AllE[T](1),
        5 (     isHappy(a)                          )   by AllE[T](2),
        6 (     isHealthy(a) ∧ isHappy(a)           )   by AndI(4, 5)
    )),
    7 (     ∀((z: T) => isHealthy(z) ∧ isHappy(z))  )   by AllI[T](3)
)
```

If we wish to introduce a for all statement, the pattern is:

- Open a subproof where you introduce an arbitrary/fresh individual in the domain with "Let" (in the example above, we used `a`). It MUST be a name that we have not used elsewhere in the proof. The idea is that your individual could have been anyone/anything in the domain.

- When you introduce the individual with "Let" and then open the subproof, you do NOT include a justification on that line

- If you have other for all statements available within the scope of the subproof, then it is often useful to use `AllE[T]` to plug your fresh individual into them. After all, if those statements are true for ALL individuals, then they are also true for your fresh individual.

- If you are trying to prove something of the form `∀ ((x: T) => P(x))`, then you need to reach `P(a)` by the end of the subproof. You need to show that your goal for all statement holds for your fresh individual. In our case, we wished to prove `∀((z: T) => isHealthy(z) ∧ isHappy(z))`, so we reached `isHealthy(a) ∧ isHappy(a)` by the end of the subproof. 

- After the subproof, you can use `∀i` to introduce a for-all statement for your last claim in the subproof -- that since the individual could have been anyone, then the proposition holds for ALL individuals. The `∀i` justification needs the line number of the subproof.

- When you use `AllI[T]`, it does not matter what variable you introduce into the for all statement. In the example above, we introduced `∀((z: T)` -- but that was only to match the goal conclusion in the proof. We could have instead introduced `∀((x: T)`, `∀((y: T)`, `∀((people T)`, etc. We would use whatever variable we chose in the rest of that proposition -- i.e., `∀((z: T) => isHealthy(z) ∧ isHappy(z))`, or `∀((people: T) => isHealthy(people) ∧ isHappy(people))`, etc.

## Examples

In this section, we will look at additional proofs involving the universal quantifier.

### Example 1

Suppose we wish to prove that, given the following premises in the domain of people:

- All students have a phone and/or a laptop
- Everyone is a student

Then we can conclude:

- Everyone has a phone and/or a laptop

First, we identify the following predicates:

- `isStudent(x)` - whether person x is a student
- `hasPhone(x)` - whether person x has a phone
- `hasLaptop(x)` = whether person x has a laptop

Then, we can translate our premises and goal conclusion to predicate logic:

- *All students have a phone and/or a laptop* translates to: `∀ x (isStudent(x) → hasPhone(x) ∨ hasLaptop(x))`
- *Everyone is a student* translates to: `∀ x isStudent(x)`
- *Everyone has a phone and/or a laptop* translates to: `∀ x (hasPhone(x) ∨ hasLaptop(x))`

We need to prove the following sequent (where we rewrite the above predicate logic statements in our Logika format):

```text
    (
        ∀((x: T) => (isStudent(x) → hasPhone(x) ∨ hasLaptop(x))), 
        ∀((x: T) => isStudent(x))
    ) 
⊢ 
    (
        ∀((y: T) => (hasPhone(y) ∨ hasLaptop(y)))
    )
```

As with our previous example, we see that we are trying to prove a for-all statement (`∀((y: T) => (hasPhone(y) ∨ hasLaptop(y)))`). This means we will need to open a subproof and introduce a fresh individual -- perhaps `bob`. By the end of the subproof, we must show that our goal for-all statement holds for that individual -- that `hasPhone(bob) ∨ hasLaptop(bob)`. We start the proof as follows:

```text
    (
        ∀((x: T) => (isStudent(x) → hasPhone(x) ∨ hasLaptop(x))), 
        ∀((x: T) => isStudent(x))
    ) 
⊢ 
    (
        ∀((y: T) => (hasPhone(y) ∨ hasLaptop(y)))
    )
Proof (
    1 (     ∀((x: T) => (isStudent(x) → hasPhone(x) ∨ hasLaptop(x)))        )   by Premise,
    2 (     ∀((x: T) => isStudent(x))                                       )   by Premise,

    3 Let ( (bob: T) => SubProof(
        
        //goal: hasPhone(bob) ∨ hasLaptop(bob)
    )),
    
    //use AllI to conclude ∀((y: T) => (hasPhone(y) ∨ hasLaptop(y)))
)
```

We have two available for-all statements within the subproof -- `∀((x: T) => (isStudent(x) → hasPhone(x) ∨ hasLaptop(x)))` and `∀((x: T) => isStudent(x))`. Since those propositions hold for all individuals, they also hold for `bob`. We use `AllE[T]` to plug in `bob` to those two propositions:

```text
    (
        ∀((x: T) => (isStudent(x) → hasPhone(x) ∨ hasLaptop(x))), 
        ∀((x: T) => isStudent(x))
    ) 
⊢ 
    (
        ∀((y: T) => (hasPhone(y) ∨ hasLaptop(y)))
    )
Proof (
    1 (     ∀((x: T) => (isStudent(x) → hasPhone(x) ∨ hasLaptop(x)))        )   by Premise,
    2 (     ∀((x: T) => isStudent(x))                                       )   by Premise,

    3 Let ( (bob: T) => SubProof(
        4 (     isStudent(bob) → hasPhone(bob) ∨ hasLaptop(bob)             )   by AllE[T](1),
        5 (     isStudent(bob)                                              )   by AllE[T](2),

        //goal: hasPhone(bob) ∨ hasLaptop(bob)
    )),
    
    //use AllI to conclude ∀((y: T) => (hasPhone(y) ∨ hasLaptop(y)))
)
```

Line 5 is an implies statement the form `p → q`, and line 6 is a statement of the form `p`. Thus we can use `→e` to conclude `hasPhone(bob) ∨ hasLaptop(bob)` (the "q" in that statement) -- which is exactly what we needed to end the subproof. All that remains is to apply our `AlI` rule after the subproof. Here is the completed proof:

```text
    (
        ∀((x: T) => (isStudent(x) → hasPhone(x) ∨ hasLaptop(x))), 
        ∀((x: T) => isStudent(x))
    ) 
⊢ 
    (
        ∀((y: T) => (hasPhone(y) ∨ hasLaptop(y)))
    )
Proof (
    1 (     ∀((x: T) => (isStudent(x) → hasPhone(x) ∨ hasLaptop(x)))        )   by Premise,
    2 (     ∀((x: T) => isStudent(x))                                       )   by Premise,

    3 Let ( (bob: T) => SubProof(
        4 (     isStudent(bob) → hasPhone(bob) ∨ hasLaptop(bob)             )   by AllE[T](1),
        5 (     isStudent(bob)                                              )   by AllE[T](2),
        6 (     hasPhone(bob) ∨ hasLaptop(bob)                              )   by ImplyE(4, 5)
    )),
    7 (     ∀((y: T) => (hasPhone(y) ∨ hasLaptop(y)))                       )   by AllI[T](3)
)
```

### Example 2

Next, suppose we wish to prove the following sequent:

```text
    (
        ∀((x: T) => (S(x) → Pz(x))),
        ∀((x: T) => (Pz(x) → D(x))),
        ∀((x: T) => ¬D(x))
    )
⊢
    (
        ∀((x: T) => ¬S(x))
    )
```

We again see that the top-level operator of what we are trying to prove is a universal quantifier. We use our strategy to open a subproof with a fresh individual (maybe `a`), and plug that individual into any available for-all statements. Since we wish to prove `∀((x: T) => ¬S(x))`, then we will want to reach `¬S(a)` by the end of the subproof. Here is a sketch:

```text
    (
        ∀((x: T) => (S(x) → Pz(x))),
        ∀((x: T) => (Pz(x) → D(x))),
        ∀((x: T) => ¬D(x))
    )
⊢
    (
        ∀((x: T) => ¬S(x))
    )

Proof(
    1 (     ∀((x: T) => (S(x) → Pz(x)))     )   by Premise,
    2 (     ∀((x: T) => (Pz(x) → D(x))      )   by Premise,
    3 (     ∀((x: T) => ¬D(x))              )   by Premise,

    4 Let ((a: T) => SubProof (
        5 (     S(a) → Pz(a)                )   by AllE[T](1),
        6 (     Pz(a) → D(a)                )   by AllE[T](2),
        7 (     ¬D(a)                       )   by AllE[T](3),

        //goal: ¬S(a)
    )),
    //use AllI[T] to conclude ∀((x: T) => ¬S(x))
)
```

Now, we see that our goal is to reach `¬S(a)` by the end of the subproof -- so we need to prove something whose top-level operator is a NOT. We recall that we have a strategy to prove NOT(something) from propositional logic -- we open a subproof, assuming *something* (`S(a)`, in our case), try to get a contradiction, and use negation introduction after the subproof to conclude NOT (something) (`¬S(a)` for us). Here is the strategy:

```text
    (
        ∀((x: T) => (S(x) → Pz(x))),
        ∀((x: T) => (Pz(x) → D(x))),
        ∀((x: T) => ¬D(x))
    )
⊢
    (
        ∀((x: T) => ¬S(x))
    )

Proof(
    1 (     ∀((x: T) => (S(x) → Pz(x)))     )   by Premise,
    2 (     ∀((x: T) => (Pz(x) → D(x))      )   by Premise,
    3 (     ∀((x: T) => ¬D(x))              )   by Premise,

    4 Let ((a: T) => SubProof(
        5 (     S(a) → Pz(a)                )   by AllE[T](1),
        6 (     Pz(a) → D(a)                )   by AllE[T](2),
        7 (     ¬D(a)                       )   by AllE[T](3),

        8 SubProof(
            10 Assume( S(a) ),
            
            //goal: contradiction
        ),
        //use NegI to conclude ¬S(a)

        //goal: ¬S(a)
    )),
    //use AllI[T] to conclude ∀((x: T) => ¬S(x))
)
```

We can complete the proof as follows:

```text
    (
        ∀((x: T) => (S(x) → Pz(x))),
        ∀((x: T) => (Pz(x) → D(x))),
        ∀((x: T) => ¬D(x))
    )
⊢
    (
        ∀((x: T) => ¬S(x))
    )

Proof(
    1 (     ∀((x: T) => (S(x) → Pz(x)))     )   by Premise,
    2 (     ∀((x: T) => (Pz(x) → D(x))      )   by Premise,
    3 (     ∀((x: T) => ¬D(x))              )   by Premise,

    4 Let ((a: T) => SubProof(
        5 (     S(a) → Pz(a)                )   by AllE[T](1),
        6 (     Pz(a) → D(a)                )   by AllE[T](2),
        7 (     ¬D(a)                       )   by AllE[T](3),

        8 SubProof(
            10 Assume( S(a) ),
            11 (    Pz(a)                   )   by ImplyE(5, 10),
            12 (    D(a)                    )   by ImplyE(6, 11),
            13 (    F                       )   by NegE(12, 7)
            
            //goal: contradiction
        ),
        //use ¬i to conclude ¬S(a)
        14 (    ¬S(a)                       )   by NegI(8)

        //goal: ¬S(a)
    )),
    //use AllI[T] to conclude ∀((x: T) => ¬S(x))
    15 (    ∀((x: T) => ¬S(x))              )   by AllI[T](4)
)
```