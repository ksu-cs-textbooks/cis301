---
title: "Rules with ∀"
pre: "6.1. "
weight: 70
date: 2018-08-24T10:53:26-05:00
---

Just as with our propositional logic operators, there will be two inference rules for each of our predicate logic quanitifiers -- an "introduction rule and an "elimination" rule.

In this section, we will see the two inference rules for the universal (∀) quantifier.

## For all elimination

For all elimination allows us to take a claim that uses a universal quantifier -- a statement about ALL individuals in a domain -- and make the same statement about a specific individual in the domain. After all, if the statement is true for ALL individuals, then it follows that it should be true for a particular individual. We can formalize the rule as follows:

```text
     ∀ x P(x)
∀e: ----------- 
       P(v)     where v is a particular individual in the domain
```

Here is a simple example showing the syntax of the `∀e` rule. It shows that given the premises: *All humans are mortal* and *Socrates is a human*, that we can prove that *Socrates is mortal*:

```text
∀ x (isHuman(x) → isMortal(x)),  isHuman(Socrates) ⊢ isMortal(Socrates)
{
    1. ∀ x (isHuman(x) → isMortal(x))               premise
    2. isHuman(Socrates)                            premise
    3. isHuman(Socrates) → isMortal(Socrates)       ∀e 1 Socrates
    4. isMortal(Socrates)                           →e 3 2
}
```

We can read the justification `∀e 1 Socrates` as: "for all elimination of the for-all statement on line 1, plugging in the individual Socrates".

## For all introduction

If we can show that a property of the form `P(a)` holds for an arbitrary member `a` of a domain, then we can use for all introduction to conclude that the property must hold for ALL individuals in the domain -- i.e., that `∀ x P(x)`. We can formalize the fule as follows:

```text
     { a              (a is fresh)
       ... P(a) }
∀i: ---------------
      ∀ x P(x) 

```

Here is a simple example showing the syntax of the `∀i` rule: "Everyone is healthy; everyone is happ. Therefore, everyone is both healthy and happy.":

```text
∀ x isHealthy(x), ∀ y isHappy(y)  |-  ∀ z(isHealthy(z) ∧ isHappy(z))
{

  1. ∀ x isHealthy(x)                   premise
  2. ∀ y isHappy(y)                     premise
  3. {
       4. a
       5. isHealthy(a)                  ∀e 1 a
       6. isHappy(a)                    ∀e 2 a
       7. isHealthy(a) ∧ isHappy(a)     ∧i 5 6
  }
  8. ∀ z (isHealthy(z) ∧ isHappy(z))    ∀i 3
}
```

If we wish to introduce a for-all statement, the pattern is:

- Open a subproof and introduce an arbitrary/fresh individual in the domain (in the example above, we used `a`). It MUST be a name that we have not used elsewhere in the proof. The idea is that your individual could have been anyone/anything in the domain.

- When you introduce your individual, you do NOT include a justification on that line

- If you have other for-all statements available within the scope of the subproof, then it is often useful to use `∀e` to plug your fresh individual into them. After all, if those statements are true for ALL individuals, then they are also true for your fresh individual.

- If you are trying to prove something of the form `∀ x P(x)`, then you need to reach `P(a)` by the end of the subproof. You need to show that your goal for-all statement holds for your fresh individual. In our case, we wished to prove `∀ z (isHealthy(z) ∧ isHappy(z))`, so we reached `isHealthy(a) ∧ isHappy(a)` by the end of the subproof. 

- After the subproof, you can use `∀i` to introduce a for-all statement for your last claim in the subproof -- that since the individual could have been anyone, then the proposition holds for ALL individuals. The `∀i` justification needs the line number of the subproof.

- When you use `∀i`, it does not matter what variable you introduce into the for-all statement. In the example above, we introduced `∀ z` -- but that was only to match the goal conclusion in the proof. We could have instead introduced `∀ x`, `∀ y`, `∀ people`, etc. We would use whatever variable we chose in the rest of that proposition -- i.e., `∀ z (isHealthy(z) ∧ isHappy(z))`, or `∀ people (isHealthy(people) ∧ isHappy(people))`, etc.

## Examples

In this section, we will look at several proofs involving the universal quantifier.

### Example 1

Suppose we wish to prove the following sequent:

```text
∀ x P(x) ⊢ ∀ y P(y)
```

This will let us show that it doesn't matter what variable we use with a universal quantifier -- both `∀ x P(x)` and `∀ y P(y)` are saying the same thing: *for all individuals, P holds for that individual*.

Since the top-level operator of our conclusion is a for-all statement, we see that we will need to use for all introduction. Following the pattern above, we open a subproof and introduce a fresh individual, `a`. Since we wish to introduce the for-all statement `∀ y P(y)`, then we know we need to reach `P(a)` by the end of our subproof:

```text
∀ x P(x) ⊢ ∀ y P(y)
{
    1. ∀ x P(x)         premise
    2. {
        3. a            //our fresh individual

        //need: P(a)
    }
    //want to use ∀i to conclude ∀ y P(y)
}
```

Since we have an available for-all statement in our subproof (`∀ x P(x)`, from line 1), then we use `∀e` to plug `a` into it:

```text
∀ x P(x) ⊢ ∀ y P(y)
{
    1. ∀ x P(x)         premise
    2. {
        3. a            //our fresh individual
        4. P(a)         ∀e 1 a
        //need: P(a)
    }
    //want to use ∀i to conclude ∀ y P(y)
}
```

At that point, we see that we have exactly the proposition we wanted to end our subproof -- `P(a)`. All that remains is to use `∀i` to state that since `a` could have been anyone, that the proposition we reached at the end of subproof 2 must hold for all individuals. Here is the completed proof:

```text
∀ x P(x) ⊢ ∀ y P(y)
{
    1. ∀ x P(x)         premise
    2. {
        3. a            //our fresh individual
        4. P(a)         ∀e 1 a
        //need: P(a)
    }
    //want to use ∀i to conclude ∀ y P(y)
    5. ∀ y P(y)         ∀i 2
}
```

### Example 2

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

We need to prove the following sequent:

```text
∀ x (isStudent(x) → hasPhone(x) ∨ hasLaptop(x)), ∀ x isStudent(x) ⊢ ∀ x (hasPhone(x) ∨ hasLaptop(x))
```

As with our previous example, we see that we are trying to prove a for-all statement (`∀ x (hasPhone(x) ∨ hasLaptop(x))`). This means we will need to open a subproof and introduce a fresh individual -- perhaps `bob`. By the end of the subproof, we must show that our goal for-all statement holds for that individual -- that `hasPhone(bob) ∨ hasLaptop(bob)`. We start the proof as follows:

```text
∀ x (isStudent(x) → hasPhone(x) ∨ hasLaptop(x)), ∀ x isStudent(x) ⊢ ∀ x (hasPhone(x) ∨ hasLaptop(x))
{
    1. ∀ x (isStudent(x) → hasPhone(x) ∨ hasLaptop(x))          premise
    2. ∀ x isStudent(x)                                         premise
    3. {
        4. bob
        
        //goal: hasPhone(bob) ∨ hasLaptop(bob)
    }
    //use ∀i to conclude ∀ x (hasPhone(x) ∨ hasLaptop(x))
}
```

We have two available for-all statements within the subproof -- `∀ x (isStudent(x) → hasPhone(x) ∨ hasLaptop(x))` and `∀ x isStudent(x)`. Since those propositions hold for all individuals, they also hold for `bob`. We use `Ae` to plug in `bob` to those two propositions:

```text
∀ x (isStudent(x) → hasPhone(x) ∨ hasLaptop(x)), ∀ x isStudent(x) ⊢ ∀ x (hasPhone(x) ∨ hasLaptop(x))
{
    1. ∀ x (isStudent(x) → hasPhone(x) ∨ hasLaptop(x))          premise
    2. ∀ x isStudent(x)                                         premise
    3. {
        4. bob
        5. isStudent(bob) → hasPhone(bob) ∨ hasLaptop(bob)      ∀e 1 bob
        6. isStudent(bob)                                       ∀e 2 bob 
        //goal: hasPhone(bob) ∨ hasLaptop(bob)
    }
    //use ∀i to conclude ∀ x (hasPhone(x) ∨ hasLaptop(x))
}
```

Line 5 is an implies statement the form `p → q`, and line 6 is a statement of the form `p`. Thus we can use `→e` to conclude `hasPhone(bob) ∨ hasLaptop(bob)` (the "q" in that statement) -- which is exactly what we needed to end the subproof. All that remains is to apply our `∀i` rule after the subproof. Here is the completed proof:

```text
∀ x (isStudent(x) → hasPhone(x) ∨ hasLaptop(x)), ∀ x isStudent(x) ⊢ ∀ x (hasPhone(x) ∨ hasLaptop(x))
{
    1. ∀ x (isStudent(x) → hasPhone(x) ∨ hasLaptop(x))          premise
    2. ∀ x isStudent(x)                                         premise
    3. {
        4. bob
        5. isStudent(bob) → hasPhone(bob) ∨ hasLaptop(bob)      ∀e 1 bob
        6. isStudent(bob)                                       ∀e 2 bob   
        7. hasPhone(bob) ∨ hasLaptop(bob)                       →e 5 6
        //goal: hasPhone(bob) ∨ hasLaptop(bob)
    }
    //use ∀i to conclude ∀ x (hasPhone(x) ∨ hasLaptop(x))
    8. ∀ x (hasPhone(x) ∨ hasLaptop(x))                         ∀i 3
}
```

### Example 3

Next, suppose we wish to prove the following sequent:

```text
∀ x (S(x) → Pz(x)), ∀ x (Pz(x) → D(x)), ∀ x ¬D(x) ⊢ ∀ x ¬S(x)
```

We again see that the top-level operator of what we are trying to prove is a universal quantifier. We use our strategy to open a subproof, introduce a fresh individual (maybe `a`), and plug that individual into any available for-all statements. Since we wish to prove `∀ x ¬S(x)`, then we will want to reach `¬S(a)` by the end of the subproof. Here is a sketch:

```text
∀ x (S(x) → Pz(x)), ∀ x (Pz(x) → D(x)), ∀ x ¬D(x) ⊢ ∀ x ¬S(x)
{
    1. ∀ x (S(x) → Pz(x))               premise
    2. ∀ x (Pz(x) → D(x))               premise
    3. ∀ x ¬D(x)                        premise
    4. {
        5. a
        6. S(a) → Pz(a)                 ∀e 1 a
        7. Pz(a) → D(a)                 ∀e 2 a
        8. ¬D(a)                        ∀e 3 a

        //goal: ¬S(a)
    }
    //use ∀i to conclude ∀ x ¬S(x)
}
```

Now, we see that our goal is to reach `¬S(a)` by the end of the subproof -- so we need to prove something whose top-level operator is a NOT. We recall that we have a strategy to prove NOT(something) from propositional logic -- we open a subproof, assuming *something* (`S(a)`, in our case), try to get a contradiction, and use NOT introduction after the subproof to conclude NOT (something) (`¬S(a)` for us). Here is the strategy:

```text
∀ x (S(x) → Pz(x)), ∀ x (Pz(x) → D(x)), ∀ x ¬D(x) ⊢ ∀ x ¬S(x)
{
    1. ∀ x (S(x) → Pz(x))               premise
    2. ∀ x (Pz(x) → D(x))               premise
    3. ∀ x ¬D(x)                        premise
    4. {
        5. a
        6. S(a) → Pz(a)                 ∀e 1 a
        7. Pz(a) → D(a)                 ∀e 2 a
        8. ¬D(a)                        ∀e 3 a
        9. {
            10. S(a)                    assume

            //goal: contradiction
        }
        //use ¬i to conclude ¬S(a)

        //goal: ¬S(a)
    }
    //use ∀i to conclude ∀ x ¬S(x)
}
```

We can complete the proof as follows:

```text
∀ x (S(x) → Pz(x)), ∀ x (Pz(x) → D(x)), ∀ x ¬D(x) ⊢ ∀ x ¬S(x)
{
    1. ∀ x (S(x) → Pz(x))               premise
    2. ∀ x (Pz(x) → D(x))               premise
    3. ∀ x ¬D(x)                        premise
    4. {
        5. a
        6. S(a) → Pz(a)                 ∀e 1 a
        7. Pz(a) → D(a)                 ∀e 2 a
        8. ¬D(a)                        ∀e 3 a
        9. {
            10. S(a)                    assume
            11. Pz(a)                   →e 6 10
            12. D(a)                    →e 7 11
            13. ⊥                       ¬e 12 8
            //goal: contradiction
        }
        //use ¬i to conclude ¬S(a)
        14. ¬S(a)                       ¬i 9

        //goal: ¬S(a)
    }
    //use ∀i to conclude ∀ x ¬S(x)
    15. ∀ x ¬S(x)                       ∀i 4
}
```