---
title: "OR Rules"
pre: "4.3. "
weight: 52
date: 2018-08-24T10:53:26-05:00
---

In this section, we will see the deduction rules for the OR operator.

## OR introduction

If we know that a proposition `P` is true, then it will also be the case that both `P ∨ Q` and `Q ∨ P` are also true. It doesn't matter what `Q` is -- it might even be something that is know to be false. Because `P` is true, it will make the overall OR statement true as well.

There are two OR introduction rules -- `OrI1` and `OrI2`. `OrI1` allows us to claim an OR statement with some previous fact on the left (first) side, and `OrI2` allows us to do the same with the right (second) side. Here is the formalization of each rule: 

```text
          P                   Q     
OrI1 : --------    OrI2 :  -------- 
        P ∨ Q               P ∨ Q 
```

Here is a simple example showing the syntax of the `OrI1` rule:

```text
(p) ⊢ (p ∨ q)
    Proof(
        1 (     p          )       by Premise,
        2 (     p ∨ q      )       by OrI1(1)
    )
```

We can read the justification `OrI(1)` as: OR introduction 1 from line 1, or "create a OR statement that puts the claim from line 1 on the first (left) side, and puts something new on the second side".

Here is a simple example showing the syntax of the `OrI2` rule:

```text
(p) ⊢ (q ∨ p)
    Proof(
        1 (     p          )       by Premise,
        2 (     q ∨ p      )       by OrI2(1)
    )
```

We can read the justification `OrI2(1)` as: OR introduction 2 from line 1, or "create a OR statement that puts the claim from line 1 on the second (right) side, and puts something new on the first side".

## OR elimination

The OR elimination rule is used when we have an OR statement of the form `P ∨ Q`, and we wish to use it to extract new information. In real life, we call the rule "case analysis". For example, say that you have either 12 quarters in your pocket or 30 dimes in your pocket. In either case, you can buy a $3.00 coffee. Why? You do a case analysis:

- In the case you have 12 quarters, that totals $3.00, and you can buy the coffee;
- In the case you have 30 dimes, that totals $3.00, and you can buy the coffee.

So, in both cases, you can buy the coffee.

We can formalize the idea behind the OR elimination rule as follows:

- In order for the OR statement `P ∨ Q` to be true, at least one of `P` and `Q` must be individually true
- If we are able to reach some conclusion `R` if we assume `P` is true, and we are able to reach the SAME conclusion `R` if we assume `Q` is true...
- ...Then no matter what, `R` will be true.

### Subproofs

OR elimination will be our first proof rule that uses subproofs. Subproofs are tools for case analysis or what-if excursions, used to support justification for later claims. In propositional logic, they will always contain one assumption. This assumption is a proposition whose scope is limited to the subproof. The syntax of a subproof in Logika looks like this:

```text
(premises) ⊢ (conclusion)
    Proof(
        ...
        5 (     fact_A             )   by Justification_A,
        ...
        17 SubProof(
            18 Assume(  fact_D  ),
            ...
            25 (    fact_G        )   by (some justification using claim 5)   //this is OK
            ...
        ),
        ...
        45 (    fact_R            )   by (some justification using claim 25)  //this is NOT OK
        ...
    )
```

Opening and closing parentheses, `(...)`, that are attached to "Proof" and "SubProof" elements define the scope of claims. The SubProof elements are given a claim number when they are opened, but no justification. Closing parenthesis for ending proofs and subproofs are not given claim numbers. The use of parentheses in this manner is analogous to the use of curly brackets use to define scope in Java, C# and C.

In the example above, the subproof starting on line 17 creates an environment where fact_D is true. The justification used on claim number 25, which uses claim 15, is valid. The scope of claim 5 includes subproof 17.

However, the justification for line number 45 is invalid. Fact_G on line number 25 was proven true in an environment where fact_D is true (ie subproof 17).  That environment ends (falls out of scope) when the closing parenthesis for the subproof is reached. This happens before line 45.

Only specific deduction rules allow you to close a scope and create a new claim based on that subproof in the enclosing (outer) scope. These rules always take a subproof (i.e "17") as part of the justification.

### Syntax

Here is the OR elimination rule:

```text
                    SubProof(              SubProof(
                        Assume ( P ),           Assume ( Q ),
      P ∨ Q             ...                     ...
                        R                       R
                    ),                     ),
OrE : -----------------------------------------------------------
                         R
```

In order to use the `OrE` rule, we must have three things:

- An OR statement of the form `P ∨ Q`
- A subproof that begins by assuming the left side of the OR statement (`P`) and ends with some claim `R`
- A subproof that begins by assuming the right side of the OR statement (`Q`) and ends with the same claim `R`

If we have all three parts, we can use the `OrE` and cite the OR statement and both subproofs to claim that `R` is true no matter what.

Here is a simple example showing the syntax of the `OrE` rule:

```text
(p ∨ q) ⊢ (q ∨ p)
    Proof(
        1 (     p ∨ q          )           by Premise,

        2 SubProof(
            3 Assume (  p  ),
            4 (     q ∨ p      )           by OrI12(3)
        ),
        5 SubProof(
            6 Assume (  q   ),
            7 (     q ∨ p      )           by OrI1(6)
        ),
        8 (     q ∨ p          )           by OrE(1, 2, 5)
    )
```

Here, we have the OR statement `p ∨ q`. We then have two subproofs where we separately assume that the two sides or the OR are true. The first subproof on line 2 starts by assuming the left side of the OR, `p`. It then uses OR introduction to reach the goal conclusion, `q ∨ p`. After reaching our goal, we end the first subproof and immediately start a second subproof. In the second subproof, we assume that the the right side of our OR statement is true, `q`. We then use the other form of OR introduction to reach the SAME conclusion as we did in the first subproof -- `q ∨ p`. We end the second subproof and can now use `∨e` to state that our conclusion `q ∨ p` must be true no matter what. After all, we knew that at least one of `p` or `q` was true, and we were able to reach the conclusion `q ∨ p` in both cases.

When using the justification:

```text
OrE(1, 2, 5)
```

The first line number corresponds to our original OR statement (line 1 with `p ∨ q` for us), the second line number corresponds to the subproof where we assumed the first (left) side of that OR statement (line 2 for us, which starts the subproof where we assumed `p`), and the third line number corresponds to the subproof where we assumed the second (right) side of that OR statement (line 5 for us, which starts the subproof where we assumed `q`)

This proof shows that the OR operator is commutative.

## Example 1

Suppose we want to prove the following sequent:

```text
(p ∧ (q ∨ r)) ⊢ ((p ∧ q) ∨ (p ∧ r))
```

As we have done before, we start by extracting whatever we can from our premises:

```text
(p ∧ (q ∨ r)) ⊢ ((p ∧ q) ∨ (p ∧ r))
    Proof(
        1 (     p ∧ (q ∨ r)        )   by Premise,
        2 (     p                  )   by AndE1(1),
        3 (     q ∨ r              )   by AndE2(1),
        ...
    )
```

Next, we look at what we are trying to prove, and see that its top-level operator is an OR. If we already had either side of our goal OR statement (i.e., either `p ∧ q` or `p ∧ r`), then we could use `AndI` to create the desired proposition. This isn't the case for us, though, so we need to use a different strategy.

The next consideration when we want to prove an OR statement is whether we have another OR statement, either as a premise or a fact we have already established. If we do, then we can attempt to use OR elimination with that OR statement to build our goal conclusion (`(p ∧ q) ∨ (p ∧ r)`). We have the OR statement `q ∨ r` available, so we'll try to use OR elimination -- we'll have a subproof where we assume `q` and try to reach `(p ∧ q) ∨ (p ∧ r)`, and then a subproof where we assume `r` and try to reach `(p ∧ q) ∨ (p ∧ r)`:

```text
(p ∧ (q ∨ r)) ⊢ ((p ∧ q) ∨ (p ∧ r))
    Proof(
        1 (     p ∧ (q ∨ r)            )   by Premise,
        2 (     p                      )   by AndE1(1),
        3 (     q ∨ r                  )   by AndE2(1),

        4 SubProof(
            5 Assume(  q  ),
            6 (     p ∧ q              )   by AndI(2,5),
            7 (     (p ∧ q) ∨ (p ∧ r)  )   by OrI1(6)
        ),
        8 SubProof(
            9 Assume(  r  ),
            10 (    p ∧ r             )   by AndI(2,9),
            11 (    (p ∧ q) ∨ (p ∧ r) )   by OrI2(10)
        ),
        12 (  (p ∧ q) ∨ (p ∧ r)     )   by OrE(3, 4, 8)
    )
```
We can make our final claim:

```text
12 (  (p ∧ q) ∨ (p ∧ r)     )   by OrE(3, 4, 8)
```

Because we had an OR statement on line 3 (`q ∨ r`), assumed the left side of that OR (`q`) in subproof 4 and reached the conclusion of `(p ∧ q) ∨ (p ∧ r)`, and then assumed the right side of our OR (`r`) in subproof 8 and reached the SAME conclusion of `(p ∧ q) ∨ (p ∧ r)`.

## Example 2

Suppose we want to prove the following sequent:

```text
((p ∨ q) ∧ (p ∨ r)) ⊢ (p ∨ (q ∧ r))
```

Note that this is the same as the previous example, but the premises are switched with the conclusion. If we prove this direction too, we will have shown that `(p ∨ q) ∧ (p ∨ r)` is equivalent to `p ∨ (q ∧ r)`. We'll learn more about this process in section 4.8.

We start by pulling in our premises and extracting whatever information we can:

```text
((p ∨ q) ∧ (p ∨ r)) ⊢ (p ∨ (q ∧ r))
    Proof(
        1 (     (p ∨ q) ∧ (p ∨ r)      )   by Premise,
        2 (     p ∨ q                  )   by AndE1(1),
        3 (     p ∨ r                  )   by AndE2(1),
        ...
    )
```

We see that the top-level operator of our conclusion (`p ∨ (q ∧ r)`) is an OR, so we apply the same strategy we did in the previous example -- we see if we have an OR statement already available as a claim, and then try to use OR elimination on it to build to our conclusion in both subproofs. In this case, though, we have TWO or statements -- `p ∨ q` and `p ∨ r`. We will see that it doesn't matter which of these we choose, so let's pick the first one -- `p ∨ q`:

```text
((p ∨ q) ∧ (p ∨ r)) ⊢ (p ∨ (q ∧ r))
    Proof(
        1 (     (p ∨ q) ∧ (p ∨ r)       )   by Premise,
        2 (     p ∨ q                   )   by AndE1(1),
        3 (     p ∨ r                   )   by AndE2(1),

        4 SubProof(
            5 Assume(  p  ),
            6 (     p ∨ (q ∧ r)         )   by OrI1(5)
        )
        7 SubProof(
            8 Assume(   q               ),
            
            //what do we do now?
        )
        ...
    )
```

We are able to finish the first subproof, but it's not clear what to do in the second subproof. We assume `q`, and know we have the goal of reaching the same conclusion as we did in the first subproof, `p ∨ (q ∧ r)`...but we don't have enough information yet to get there. The only piece of information that we haven't used that might help us is our second OR statement -- `p ∨ r`. We are already inside a subproof, but we can still nest other subproofs -- just as we can nest conditional statements in computer programs.

We start on a nested OR elimination approach with `p ∨ r`:

```text
((p ∨ q) ∧ (p ∨ r)) ⊢ (p ∨ (q ∧ r))
    Proof(
        1 (     (p ∨ q) ∧ (p ∨ r)       )   by Premise,
        2 (     p ∨ q                   )   by AndE1(1),
        3 (     p ∨ r                   )   by AndE2(1),

        4 SubProof(
            5 Assume(  p  ),
            6 (     p ∨ (q ∧ r)         )   by OrI1(5)
        ),
        7 SubProof(
            8 Assume(  q  ),
            
            //start OR elimination with p ∨ r
            9 SubProof(
                10 Assume(  p  ),
                11 (    p ∨ (q ∧ r)     )   by OrI1(10)         
            ),
            12 SubProof(
                13 Assume(  r  ),
                14 (    q ∧ r           )   by AndI(8, 13),
                15 (    p ∨ (q ∧ r)     )   by OrI2(14)
            ),
            16 (    p ∨ (q ∧ r)         )   by OrE(3, 9, 12)
        ),
        17 (    p ∨ (q ∧ r)             )   by OrE(2, 4, 7)
    )
```

Note that we use the ∨e rule twice -- on line 16 to tie together subproofs 9 and 12 (where we processed the OR statement `p ∨ r`), and on line 17 to tie together subproofs 4 and 7 (where we processed the OR statement `p ∨ q`).

When we first started this problem, we mentioned that it didn't matter which OR statement we chose to work with -- `p ∨ q` or `p ∨ r`. Indeed, we could have chosen `p ∨ r` instead -- but we would end up nesting another OR elimination for `p ∨ q`:

```text
((p ∨ q) ∧ (p ∨ r)) ⊢ (p ∨ (q ∧ r))
    Proof(
        1 (     (p ∨ q) ∧ (p ∨ r)      )   by Premise,
        2 (     p ∨ q                  )   by AndE1(1),
        3 (     p ∨ r                  )   by AndE2(1),
        
        //OR elimination for p ∨ r
        4 SubProof(
            5 Assume(  p  ),
            6 (     p ∨ (q ∧ r)        )   by OrI1(5)
        ),
        7 SubProof(
            8 Assume(  r  ),
            
            //start OR elimination with p ∨ q
            9 SubProof(
                10 Assume(  p  ),
                11 (    p ∨ (q ∧ r)   )   by OrI1(10)         
            ),
            12 SubProof(
                13 Assume(  q  ),
                14 (    q ∧ r         )   by AndI(13, 8),
                15 (    p ∨ (q ∧ r)   )   by OrI2(14)
            ),
            16 (    p ∨ (q ∧ r)       )   by OrE(2, 9, 12)
        ),
        17 (    p ∨ (q ∧ r)           )   by OrE(3, 4, 7)
    )
```