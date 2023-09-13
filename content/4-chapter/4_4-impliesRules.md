---
title: "Implies Rules"
pre: "4.4. "
weight: 53
date: 2018-08-24T10:53:26-05:00
---

In this section, we will see the deduction rules for the implies operator.

## Implies elimination

Remember that `→` is a kind of logical "if-then". Here, we understand `p → q` to mean that `p` holds knowledge sufficient to deduce `q` – so, whenever `p` is proved to be a fact, then `p → q` enables `q` to be proved a fact, too. This is the implies elimination rule, `→e`, and we can formalize it like this:

```text
       P → Q    P
 →e :  ------------
             Q
```

Here is a simple example of a proof that shows the syntax of the `→e` rule:

```text
a, a → b ⊢ b
{
    1. a                    premise
    2. a → b                premise
    3. b                    →e 2 1
}
```

Note that when we use `→e`, we first list the line number of the implies statement, and then list the line number that contains the left side of that implies statement. The  `→e` allows us to claim the right side of that implies statement. 

## Implies introduction

The idea behind the next deduction rule, implies introduction, is that we would be introducing a new implies statement of the form `P → Q`. In order to do this, we must be able to show our logical "if-then" -- that IF `P` exists, THEN we promise that `Q` will also exist. We do this by way of a subproof where we assume `P`. If we can reach `Q` by the end of that subproof, we will have shown that anytime `P` is true, then `Q` is also true. We will be able to close the subproof by introducing `P → Q` with the `→i` rule. We can formalize the rule like this:

```text
       { P assume   
         ... Q    } 
→i : -------------- 
          P → Q 
```

Here is a simple example of a proof that shows the syntax of the `→i` rule:

```text
a → b, b → c ⊢ a → c
{
    1. a → b               premise
    2. b → c               premise
    3. {
        //we want to prove a → c, so we start by assuming a

        4. a                assume
        5. b                →e 1 4
        6. c                →e 2 5

        //...and try to end with c
    }
    //then we can conclude that anytime a is true, then c is also true

    7. a → c                →i 3
}
```
Note that when we use `→i`, we list the line number of the subproof we just finished. We must have started that subproof by assuming the left side of the implies statement we are introducing, and ended that subproof with the right side of the implies statement we are introducing.


## Example 1

Suppose we want to prove the following sequent:

```text
p → (q → r) ⊢ (q ∧ p) → r
```

We start by listing our premise:

```text
p → (q → r) ⊢ (q ∧ p) → r
{
    1. p → (q → r)          premise
}
```

We can't extract any information from the premise, so we shift to examining our conclusion. The top-level operator of our conclusion is an implies statement, so this tells us that we will need to use the `→i` rule. We want to prove `(q ∧ p) → r`, so we need to show that whenever `q ∧ p` is true, then `r` is also true. We open a subproof and assume the left side of our goal implies statement (`q ∧ p`). If we can reach `r` by the end of the subproof, then we can use `→i` to conclude `(q ∧ p) → r`:

```text
p → (q → r) ⊢ (q ∧ p) → r
{
    1. p → (q → r)          premise
    2. {
        3. q ∧ p            assume

        //goal: get to r
    }
    //use →i to conclude (q ∧ p) → r
}
```

Now we can complete the proof:

```text
p → (q → r) ⊢ (q ∧ p) → r
{
    1. p → (q → r)          premise
    2. {
        3. q ∧ p            assume
        4. q                ∧e1 3
        5. p                ∧e2 3
        6. q → r            →e 1 5
        7. r                →e 6 4 
        //goal: get to r
    }
    //use →i to conclude (q ∧ p) → r

    8. (q ∧ p) → r          →i 2
}
```

## Example 2

Suppose we want to prove the following sequent:

```text
p → (q → r) ⊢ (p → q) → (p → r)
```

We see that we will have no information to extract from the premises. The top-level operator is an implies statement, so we start a subproof to introduce that implies statement. In our subproof, we will assume the left side of our goal implies statement (`p → q`) and will try to reach the right side of our goal (`p → r`):

```text
p → (q → r) ⊢ (p → q) → (p → r)
{
    1. p → (q → r)          premise
    2. {
        3. p → q            assume

        //goal: get to p → r
    }
    //use →i to conclude (p → q) → (p → r)
}
```

We can see that our goal is to reach `p → r` in our subproof -- so we see that we need to introduce another implies statement. This tells us that we need to nest another subproof -- in this one, we'll assume the left side of our *current* goal implies statement (`p`), and then try to reach the right side of that current goal (`r`). Then, we'd be able to finish that inner subproof by using `→i` to conclude `p → r`:

```text
p → (q → r) ⊢ (p → q) → (p → r)
{
    1. p → (q → r)          premise
    2. {
        3. p → q            assume

        4. {
            5. p            assume

            //goal: get to r
        }
        //use →i to conclude p → r

        //goal: get to p → r
    }
    //use →i to conclude (p → q) → (p → r)
}
```

Now we can complete the proof:

```text
p → (q → r) ⊢ (p → q) → (p → r)
{
    1. p → (q → r)          premise
    2. {
        3. p → q            assume

        4. {
            5. p            assume
            6. q            →e 3 5
            7. q → r        →e 1 5 
            8. r            →e 7 6  

            //goal: get to r
        }
        //use →i to conclude p → r

        9. p → r             →i 4   

        //goal: get to p → r
    }
    //use →i to conclude (p → q) → (p → r)

    10. (p → q) → (p → r)   →i 2 
}
```

## Example 3

Here is one more example, where we see we can nest a `→i` subproof and a `∨e` subproof:

```text
p → r, q → r ⊢ (p ∨ q) → r
{
    1. p → r                premise
    2. q → r                premise
    3. {
        //assume p ∨ q, try to get to r 

        4. p ∨ q             assume

        //nested subproof for OR elimination on p ∨ q
        //try to get to r in both cases
        5. {
            6. p            assume
            7. r            →e 1 6
        }
        8. {
            9.  q           assume
            10. r           →e 2 9
        }
        11. r                ∨e 4 5 8

        //goal: get to r
    }
    //use →i to conclude (p ∨ q) → r

    12. (p ∨ q) → r         →i 3
}
```