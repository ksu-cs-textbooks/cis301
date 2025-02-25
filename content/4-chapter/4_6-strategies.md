---
title: "Summary and Strategies"
pre: "4.6. "
weight: 55
date: 2018-08-24T10:53:26-05:00
---

When examining more complex propositional logic sequents, it can be challenging to know where to start. In this section, we summarize all available rules in propositional logic, and discuss stratgies for approaching proofs.

## AND rules

Rule summaries:

```text
         P   Q                  P ∧ Q                  P ∧ Q  
AndI :  ---------     AndE1 : ----------     AndE2 : ---------- 
         P ∧ Q                    P                      Q
```

Rule syntax summaries:

```text 
    ...
    x (     p       )		by (...),
    y (     q	    )       by (...),
    z (     p ∧ q   )       by AndI(x, y),
    ...
```

```text
    ...
    x (     p ∧ q   )		by (...),
    y (     p	    )       by AndE1(x),
    z (     q       )       by AndE2(y),
    ...
```

## OR rules

Rule summaries:

```text
                                                           SubProof(                SubProof(
                                                                Assume ( P ),           Assume (Q ),
                                                 P ∨ Q          ...                     ...
                                                                R      ... R   }        R
           P                   Q                           ),                       ),
OrI1 : ---------    OrI2 : ----------      OrE : ------------------------------------------------------- 
        P ∨ Q               P ∨ Q                                     R
```

Rule syntax summaries:

```text
    ...
    x (     p       )		by (...),
    y (     p ∨ q   )       by OrI1(x),
    ...
```

```text
    ...
    x (     q       )		by (...),
    y (     p ∨ q   )       by OrI2(x),
    ...
```

```text
    ...
    a (     p ∨ q       )   by (...),
    
    b SubProof(
        c Assume(  p  ),
        ...
        d (     r       )   by (...)
    ),
    f SubProof(
        g Assume(  q  ),
        ...
        h (     r       )   by (...)
    ),
    i (     r           )   by OrE(a, b, f),
```

## Implies rules

Rule summaries:

```text
                                     SubProof(
                                        Assume( P ),
                                        ...
                                        Q 
           P → Q    P                ),
 ImplyE : -------------     ImplyI : ----------------    
           Q                           P → Q   
```

Rule syntax summaries:

```text
    ...
    x (     p → q   )   by (...),	
    y (     p       )   by (...),     
    z (     q       )   by ImplyE(x, y),
    ...
```

```text
    ...
    a SubProof(
        b Assume(  p  ),
        ...
        c (     q       )   by (...)
    ),
    d (     p → q       )   by ImplyI(a),
    ...
```

## Negation rules

Rule summaries:

```text
                  
        P   ¬P     
NegE : ----------  
          F   



        SubProof(
            Assume ( P ),
            ...
            F
        ),
NegI : --------------
           ¬P     



              F
BottomE :  ------  for any proposition, Q, at all
              Q



        SubProof(
             Assume( ¬P ),
             ...
             F   
        ),
PbC:   --------------------
               P
```

Rule syntax summaries:

```text
    ...
    x (     p       )   by (...),  
    y (     ¬p      )   by (...),
    z (     F       )   by NegE(x, y),
    ...
```

```text
    ...
    a SubProof(
        b Assume(  p  ),
        ...
        c (     F       )   by (...)
    ),
    d (     ¬p          )   by NegI(a),
    ...
```

```text
    ...
    x (     F       )   by (...),
    y (     q       )   by BottomE(x),
    ...
```

```text
    ...
    a SubProof(
        b Assume(  ¬p  ),
        ...
        c (     F       )   by (...)
    ),
    d (     p           )   by PbC(a),
    ...
```

## Strategies

1. Write down all premises first. Can you extract anything from the premises? 
	- If you have `p∧q`, use `AndE1` to extract `p` by itself and then `AndE2` to extract `q` by itself.
	- If you have `p→q` and `p`, use `ImplyE` to get `q`.
    - If you have `p` and `¬p`, use `NegE` to claim a contradiction, `F`.
2. Look at the top-level operator of what you are trying to prove.
    - Are you trying to prove something of the form `p→q`? 
        - Use `ImplyI`. Open a subproof, assume `p`, and get to `q` by the end of the subproof. After the subproof, use `ImplyI` to conclude `p→q`.

    - Are you trying to prove something of the form `¬p`?
        - Use `NegI`. Open a subproof, assume `p`, and get a contradiction, `F`, by the end of the subproof. After the subproof, use `NegI` to conclude `¬p`.

    - Are you trying to prove something of the form `p ∧ q`? 
        - Try to prove `p` by itself and then `q` by itself. Afterward, use `AndI` to conclude `p ∧ q`.

    - Are you trying to prove something of the form `p ∨ q`?
        - See if you have either `p` or `q` by itself -- if you do, use either `OrI1` or `OrI2` to conclude `p ∨ q`.
3. You'll need to nest the approaches from step 2. Once you are in a subproof, think about what you are *trying to prove by the end of that subproof*. Follow the strategy in step 2 to prove your current goal, nesting subproofs as needed. As you work, stop and scan the propositions that you have available. See if you can extract anything from them as you did for the premises in step 1.
4. No match, or still stuck?
    - Do you have an OR statement available? Try using OR elimination to prove your goal conclusion.
    - Do your propositions have NOT operators, but don't fit the form for using `¬i`? Try using `PbC`. If you are trying to prove something of the form `p`, open a subproof, assume `¬p`, and try to reach a contradiction by the end of the subproof. Afterward, use `PbC` to conclude `p`.
    - As a last resort, try pasting in the proof for the law of the excluded middle (see section 4.5). Then use OR elimination on `p ∨ ¬p`.


Proofs can be quite challenging. You might follow one approach, get stuck, and not be able to make progress. If this happens, backtrack and follow a different approach. If you are using Logika to verify your work, make sure it does not mark any lines in the proof in red – this means that you've made an invalid conclusion along the way, or that your justification for a particular line doesn't follow the expected format. Try to fix these errors before continuing on with the proof.
