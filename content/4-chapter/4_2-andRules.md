---
title: "AND Rules"
pre: "4.2. "
weight: 51
date: 2018-08-24T10:53:26-05:00
---

In this section, we will see the deduction rules for the AND operator.

## AND introduction

Clearly, when both `p` and `q` are facts, then so is the proposition `p ∧ q`. This makes logical sense -- if two propositions are independently true, then their conjunction (AND) must also be true. The AND introduction rule, `AndI`, formalizes this:

```text
        P   Q   
AndI :  ---------   
        P ∧ Q
```

We will use the format above when introducing each of our natural deduction rules:

- `P` and `Q` are not necessarily individual variables -- they are placeholders for some propositional statement, which may itself involve several logical operators.
- On the left side is the rule name (in this case, `AndI`)
- On the top of the right side we see what we already need to have established as facts in order to use this rule (in this case, `P` and also `Q` ). These facts can appear anywhere in our scope of the proof, in whatever order. (For now, all previous lines in the proof will be within our scope, but this will change when we get to more complex rules that involve subproofs).
- On the bottom of the right side, we see what we can claim by using that proof rule.

Here is a simple example of a proof that uses `AndI`. It proves that if propositional atoms `p`, `q`, and `r` are all true, then the proposition `r ∧ (q ∧ p)` is also true:

```text
(p, q, r)  ⊢  (r ∧ (q ∧ p))
    Proof(
        1 (     p              )   by Premise,
        2 (     q              )   by Premise,
        3 (     r              )   by Premise,
        4 (     q ∧ p          )   by AndI(2, 1),
        5 (     r ∧ (q ∧ p)    )   by AndI(3, 4)
    )
```

You can read line 4 like this: "from the fact `q` stated on line 2 and the fact `p` stated on line 1, we deduce `q ∧ p` by applying the `AndI` rule". Lines 4 and 5 construct new facts from the starting facts (premises) on lines 1-3.

Note that if I had instead tried:

```text
(p, q, r)  ⊢  (r ∧ (q ∧ p))
    Proof(
        1 (     p              )   by Premise,
        2 (     q              )   by Premise,
        3 (     r              )   by Premise,
        4 (     q ∧ p          )   by AndI(1, 2),
        ...
    )
```

Then line 4 would not have been accepted. The line numbers cited after the `AndI` rule must match the order of the resulting AND statement. The left-hand side of our resulting AND statement must correspond to the first line number in `AndI` justification, and the right-hand side of our resulting AND statement must correspond to the second line number in the justification:

```text
    ...
    4 (     p           )   by (some justification),
    5 (     q	        )   by (some justification),
    6 (     p ⋀ q       )   by AndI(4, 5), 
    ...
    9 (     q ⋀ p       )   by AndI(5, 4),
    ...
```


## AND elimination

The idea of the AND elimination rules is that if we have a proposition `p ⋀ q` as a fact, then we can separately claim both `p` and `q` as individual facts. After all, the only time `p ⋀ q` is true in a truth table is when both `p` and `q` are individually true. There are two AND elimination rules -- `AndE1` and `AndE2`. `AndE1` allows us to claim that the left (first) side of an AND statement is individually true, and `AndE2` allows us to do the same with the right (second) side. Here is the formalization of each rule: 

```text
        P ∧ Q                   P ∧ Q
AndE1 : ---------      AndE2 : ---------
          P                       Q
```

Here is a simple example showing the syntax of the `AndE1` rule:

```text
(p ∧ q)  ⊢  (p)
    Proof(
        1 (     p ∧ q          )   by Premise,
        2 (     p              )   by AndE1(1)
        ...
    )
```

We can read the justification `AndE1(1)` as: AND-elimination 1 from line 1, or "take the AND statement on line 1 and extract its first (left) side".


Here is a simple example showing the syntax of the `∧e2` rule:

```text
(p ∧ q)  ⊢  (q)
    Proof(
        1 (     p ∧ q          )   by Premise,
        2 (     q              )   by AndE2(1)
        ...
    )
```

We can read the justification `AndE2(1)` as: AND-elimination 2 from line 1, or "take the AND statement on line 1 and extract its second (right) side".

## Example 1

Suppose we want to prove the following sequent:

```text
(p ∧ (q ∧ r)) ⊢ (r ∧ p)
```

Whenever we approach a proof, a good first strategy is to see what we can extract from the premises. If we have a premise that is an AND statement, then we can use `AndE1` and then `AndE2` to extract both its left and right side as separate claims. So we start our proof like this:

```text
(p ∧ (q ∧ r)) ⊢  (r ∧ p)
    Proof(
        1 (     p ∧ (q ∧ r)        )       by Premise,
        2 (     p                  )       by AndE1(1),
        3 (     q ∧ r              )       by AndE2(1),
        ...
    )
```

But now we have a new AND statement as a claim -- `q ∧ r`. We can again use both `AndE1` and `AndE2` to extract each side separately:

```text
(p ∧ (q ∧ r)) ⊢  (r ∧ p)
    Proof(
        1 (     p ∧ (q ∧ r)        )       by Premise,
        2 (     p                  )       by AndE1(1),
        3 (     q ∧ r              )       by AndE2(1),
        4 (     q                  )       by AndE1(3),
        5 (     r                  )       by AndE2(3),
        ...
    )
```

Now that we have done all we can with our premises and the resulting statements, we examine our conclusion. Whenever our conclusion is a conjunction (AND statement), we know that we must separately show both the left side and the right side of that conclusion. Then, we can use `AndI` to put those sides together into our goal AND statement.

In this example, we have already proved both sides of our goal AND statement -- `r` (from line 5) and `p` (from line 2). All that remains is to use `AndI` to put them together:

```text
(p ∧ (q ∧ r)) ⊢  (r ∧ p)
    Proof(
        1 (     p ∧ (q ∧ r)        )       by Premise,
        2 (     p                  )       by AndE1(1),
        3 (     q ∧ r              )       by AndE2(1),
        4 (     q                  )       by AndE1(3),
        5 (     r                  )       by AndE2(3),
        6 (     r ∧ p              )       by AndI(5, 2)
    )
```


## Example 2

Suppose we want to prove the following sequent:

```text
(p ∧ q ∧ r, a ∧ (t ∨ s)) ⊢ (q ∧ (t ∨ s))
```

We again try to use AND-elimination to extract what we can from our premises. We might try something like this:

```text
(p ∧ q ∧ r, a ∧ (t ∨ s)) ⊢ (q ∧ (t ∨ s))
    Proof(
        1 (     p ∧ q ∧ r          )       by Premise,
        2 (     a ∧ (t ∨ s)        )       by Premise,
        3 (     p                  )       by AndE1(1),        //NO! Won't work.
        ...
    )
```

However, we get into trouble when we try to use `AndE1` to extract the left side of the premise `p ∧ q ∧ r`. The problem has to do with operator precedence -- we recall that `∧` operators are processed from left to right, which means that `p ∧ q ∧ r` is equivalent to `(p ∧ q) ∧ r`. By reminding ourselves of the "hidden parentheses", we see that when we use `AndE1` on the premise `p ∧ q ∧ r`, we extract `p ∧ q`. Similarly, `AndE2` will extract `r`.

We try again to extract what we can from our premises:

```text
(p ∧ q ∧ r, a ∧ (t ∨ s)) ⊢ (q ∧ (t ∨ s))
    Proof(
        1 (     p ∧ q ∧ r          )       by Premise,
        2 (     a ∧ (t ∨ s)        )       by Premise,
        3 (     p ∧ q              )       by AndE1(1),   
        4 (     r                  )       by AndE2(1),
        5 (     a                  )       by AndE1(2),
        6 (     t ∨ s              )       by AndE2(2),
        ...
    )
```

As before, we look at our resulting claims -- we see a `p ∧ q`, and we know that we can use AND elimination again to extract both sides. Now we have:

```text
(p ∧ q ∧ r, a ∧ (t ∨ s)) ⊢ (q ∧ (t ∨ s))
    Proof(
        1 (     p ∧ q ∧ r          )       by Premise,
        2 (     a ∧ (t ∨ s)        )       by Premise,
        3 (     p ∧ q              )       by AndE1(1),   
        4 (     r                  )       by AndE2(1),
        5 (     a                  )       by AndE1(2),
        6 (     t ∨ s              )       by AndE2(2),
        7 (     p                  )       by AndE1(3),
        8 (     q                  )       by AndE2(3),
        ...
    )
```

Now, we look at what we are trying to prove -- `q ∧ (t ∨ s)`. Since its top-level operator is the AND, we know that we must separately prove `q` and `t ∨ s`. Then, we can use AND introduction to put the two pieces together to match our conclusion. We see that we already have `q` on line 8 and `t ∨ s` on line 6, so we add our final line to finish the proof:

```text
(p ∧ q ∧ r, a ∧ (t ∨ s)) ⊢ (q ∧ (t ∨ s))
    Proof(
        1 (     p ∧ q ∧ r          )       by Premise,
        2 (     a ∧ (t ∨ s)        )       by Premise,
        3 (     p ∧ q              )       by AndE1(1),   
        4 (     r                  )       by AndE2(1),
        5 (     a                  )       by AndE1(2),
        6 (     t ∨ s              )       by AndE2(2),
        7 (     p                  )       by AndE1(3),
        8 (     q                  )       by AndE2(3),
        9 (     q ∧ (t ∨ s)        )       by AndI(8, 6)
    )
```

You might notice that lines 5 and 7 were not needed, as both `p` and `a` were not part of the conclusion. That's true -- we could have eliminated those steps. However, it's a good idea to extract as much information as possible while you are getting used to doing these proofs -- it doesn't hurt to have extra claims, and you may find that you end up needing them.