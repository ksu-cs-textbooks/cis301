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
        P   Q               P ∧ Q                  P ∧ Q  
∧i :  ---------     ∧e1 : ----------      ∧e2 : ---------- 
        P ∧ Q                 P                      Q
```

Logika syntax:

```text
{ 
    ...
    x. p		    (...)
    y. q			(...)
    z. p ∧ q        ∧i x y
    ...
}
```

```text
{ 
    ...
    x. p ∧ q	    (...)
    y. p            ∧e1 x
    z. q            ∧e2 x
    ...
}
```

## OR rules

Rule summaries:

```text
                                                           { P assume     { Q assume
          P                   Q                  P ∨ Q       ... R   }      ... R   }
∨i1 : ---------     ∨i2 : ----------      ∨e : --------------------------------------- 
        P ∨ Q               P ∨ Q                                R
```

Logika syntax:

```text
{ 
    ...
    x. p		    (...)
    y. p ∨ q        ∨i1 x
    ...
}
```

```text
{ 
    ...
    x. q		    (...)
    y. p ∨ q        ∨i2 x
    ...
}
```

```text
{
    ...
    a. p ∨ q        (...)
    b. {
        c. p        assume
        ...
        d. r        (...)
    }
    f. {
        g. q        assume
        ...
        h. r        (...)
    }
    i. r            ∨e a b f
    ...
}
```

## Implies rules

Rule summaries:

```text
                           { P assume 
      P → Q    P             ... Q   }
 →e : -----------     →i : ------------    
           Q                  P → Q   
```

Logika syntax:

```text
{ 
    ...
    x. p → q	        (...)
    y. p                (...)
    z. q                →e x y
}
```

```text
{ 
    ...
    a. {
        b. p            assume
        ///
        c. q            (...)
    }
    d. p → q            →i a
}
```

## Negation rules

Rule summaries:

```text
                           { P assume                             { ¬ P assume
        P   ¬ P             ... ⊥   }             ⊥e                ... ⊥     }
 ¬ e : ----------    ¬ i : -----------    ⊥e : --------     pbc:  --------------
           ⊥                   ¬ P                 Q                    P
```

Logika syntax:

```text
{
    x. p            (...)
    y. ¬ p          (...)
    z. ⊥            ¬ e x y 
}
```

```text
{
    a. {
        b. p        assume
        ...
        c. ⊥        (...)
    }
    d. ¬ p          ¬ i a
}
```

```text
{
    x. ⊥            (...)
    z. q            ⊥e x
}
```

```text
{
    a. {
        b. ¬ p      assume
        ...
        c. ⊥        (...)
    }
    d. p            pbc a
}
```

## Strategies

1. Write down all premises first. Can you extract anything from the premises? 
	- If you have `p∧q`, use `∧e1` to extract `p` by itself and then `∧e2` to extract `q` by itself.
	- If you have `p→q` and `p`, use `→e` to get `q`.
    - If you have `p` and `¬p`, use `¬e` to claim a contradiction, `⊥`.
2. Look at the top-level operator of what you are trying to prove.
    - Are you trying to prove something of the form `p→q`? 
        - Use `→i`. Open a subproof, assume `p`, and get to `q` by the end of the subproof. After the subproof, use `→i` to conclude `p→q`.

    - Are you trying to prove something of the form `¬p`?
        - Use `¬i`. Open a subproof, assume `p`, and get a contradiction, `⊥`, by the end of the subproof. After the subproof, use `¬i` to conclude `¬p`.

    - Are you trying to prove something of the form `p ∧ q`? 
        - Try to prove `p` by itself and then `q` by itself. Afterward, use `∧i` to conclude `p ∧ q`.

    - Are you trying to prove something of the form `p ∨ q`?
        - See if you have either `p` or `q` by itself -- if you do, use either `∨i1` or `∨i2` to conclude `p ∨ q`.
3. You'll need to nest the approaches from step 2. Once you are in a subproof, think about what you are *trying to prove by the end of that subproof*. Follow the strategy in step 2 to prove your current goal, nesting subproofs as needed. As you work, stop and scan the statements that you have available. See if you can extract anything from them as you did for the premises in step 1.
4. No match, or still stuck?
    - Do you have an OR statement available? Try using OR elimination to prove your goal conclusion.
    - Do your statements have NOT operators, but don't fit the form for using `¬i`? Try using `pbc`. If you are trying to prove something of the form `p`, open a subproof, assume `¬p`, and try to reach a contradiction by the end of the subproof. Afterward, use `pbc` to conclude `p`.
    - As a last resort, try pasting in the proof for the law of the excluded middle (see section 4.5). Then use OR elimination on `p ∨ ¬p`.


Proofs can be quite challenging. You might follow one approach, get stuck, and not be able to make progress. If this happens, backtrack and follow a different approach. As you are working, make sure Logika does not mark any lines in the proof in red – this means that you've made an invalid conclusion along the way, or that your justification for a particular line doesn't follow the expected format. Try to fix these errors before continuing on with the proof.
