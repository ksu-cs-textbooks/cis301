---
title: "Summary and Strategies"
pre: "6.5. "
weight: 74
date: 2018-08-24T10:53:26-05:00
---

In this section, we summarize all available rules in propositional logic, and discuss strategies for approaching proofs.

## Rules with universal quantifier (`∀`)

Rule summaries:

```text
           ∀ ((x: T) => P(x))
AllE[T]:  ---------------------
                   P(v)     where v is a particular individual in the domain (i.e, v has type T)
```

```text
            Let (   (a: T) => SubProof(
                ... 
                P(a)  
            )),
AllI[T] : -------------------------------
                 ∀ ((x: T) => P(x)) 

```

Rule syntax summaries:

```text
( 
    ...
    r (     ∀ ((x: T) => P(x))      )   by SomeJustification,
    s (     P(v)                    )   by AllE[T](r),      //where v has been previously shown to have type T    
    ...
)
```

```text
( 
    ...
    r Let (  (a: T) => SubProof(    //where a is a fresh, previously unused, individual
        ...
        t (     P(a)            )   by SomeJustification
    )),
    u (     ∀ ((x: T) => P(x))  )   by AllI[T](r),
)
```

## Rules with existential quantifier (`∃`)

Rule summaries:

```text
                   P(d)         where  d  is an individual of type T
ExistsI[T]: ---------------------
              ∃((x: T) => P(x))
```

```text
                                        Let ((a: T) => SubProof(          // where  a  is a new, fresh name
                                             Assume( P(a) ),              // a  MUST NOT appear in  Q
                ∃((x: T) => P(x))            ...
                                             Q         
                                        )),             
ExistsE[T]: ----------------------------------------------------
                     Q
```

Rule syntax summaries:

```text
(
    ...
    r (     P(d)                )   by SomeJustification,   //where d has been shown to have type T
    s (     ∃((x: T) => P(x))   )   by ExistsI[T](r),
    ...
)
```

```text
(
    ...
    b (     ∃((x: T) => P(x))       )   by SomeJustification,
    c Let ( (a: T) => SubProof(     //where a is a previously unused individual
        d Assume(  P(a)  ),
        ...
        f (     Q                   )   by SomeJustification    //where a does NOT appear in Q
    )),
    g (     Q                       )   by ExistsE[T](b, c),
    ...
)
```

## Reminder: propositional logic rules are still available

When doing proofs in predicate logic, remember that all the deduction rules from propositional logic are still available. You will often want to use the same strategies we saw there -- not introduction to prove a NOT, implies introduction to create an implies statement, OR elimination to process an OR statement, etc. 

However, keep in mind that propositional logic rules can only be used on claims without quantifiers as their top-level operator. For example, if we have the statement `∀ ( x: T) => (S(x) ∧ Pz(x)) )`, then we cannot use `AndE` -- the top-level operator is a universal quantifier, and the `∧` statement is "bound up" in that quantifier. We would only be able to use `AndE` after we had used `AllE[T]` to eliminate the quantifier.

## Strategies

1. Write down all premises first. Can you extract anything from the premises? 
    - If you have a for-all statement and an available individual, use `AndE[T]` to plug that individual into the for-all statement.
	- If you have `p∧q`, use `AndE1` to extract `p` by itself and then `AndE2` to extract `q` by itself.
	- If you have `p →: q` and `p`, use `ImplyE` to get `q`.
    - If you have `p` and `¬p`, use `NegE` to claim a contradiction, `F`.

2. Look at the top-level operator of what you are trying to prove.
    - Are you trying to prove something of the form `∀ ((x: T) => P(x))`?
        - Use `AllI[T]`. Open a subproof, introduce a fresh `a`, and get to `P(a)` by the end of the subproof. After the subproof, use `AllI[T]` to conclude `∀ ((x: T) => P(x))`.

    - Are you trying to prove something of the form `∃((x: T) => P(x))`?
        - You will often have another there-exists (`∃`) statement available as a premise or previous claim. Open a subproof, and assume an alias for the individual in your there-exists statement. Get to `∃((x: T) => P(x))` by the last line of the subproof. After the subproof, use `ExistsE[T]` to restate `∃((x: T) => P(x))`.

    - Are you trying to prove something of the form `p →: q`? 
        - Use `ImplyI`. Open a subproof, assume `p`, and get to `q` by the end of the subproof. After the subproof, use `ImplyI` to conclude `p →: q`.

    - Are you trying to prove something of the form `¬p`?
        - Use `NegI`. Open a subproof, assume `p`, and get a contradiction, `F`, by the end of the subproof. After the subproof, use `NegI` to conclude `¬p`.

    - Are you trying to prove something of the form `p ∧ q`? 
        - Try to prove `p` by itself and then `q` by itself. Afterward, use `AndI` to conclude `p ∧ q`.

    - Are you trying to prove something of the form `p ∨ q`?
        - See if you have either `p` or `q` by itself -- if you do, use either `OrI1` or `OrI2` to conclude `p ∨ q`.

3. You'll need to nest the approaches from step 2. Once you are in a subproof, think about what you are *trying to prove by the end of that subproof*. Follow the strategy in step 2 to prove your current goal, nesting subproofs as needed. As you work, stop and scan the claims that you have available. See if you can extract anything from them as you did for the premises in step 1.

4. No match, or still stuck?
    - Do you have a there-exists statement available? Try using `ExistsE[T]` to reach your goal.
    - Do you have an OR statement available? Try using OR elimination to prove your goal conclusion.
    - Do your statements have NOT operators, but don't fit the form for using `¬i`? Try using `PbC`. If you are trying to prove something of the form `p`, open a subproof, assume `¬p`, and try to reach a contradiction by the end of the subproof. Afterward, use `PbC` to conclude `p`.
    - As a last resort, try pasting in the proof for the law of the excluded middle (see section 4.5). Then use OR elimination on `p ∨ ¬p`.


Think of doing a proof as solving a maze, and of all our deduction rules as possible directions you can turn. If you have claims that match a deduction rule, then you can try applying the rule (i.e, "turning that direction"). As you work, you may apply more and more rules until the proof falls into place (you exit the maze)...or, you might get stuck. If this happens, it doesn't mean that you have done anything *wrong* -- it just means that you have reached a dead end and need to try something else. You backtrack, and try a different approach instead (try turning in a different direction).