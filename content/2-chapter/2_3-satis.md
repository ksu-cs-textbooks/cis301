---
title: "Satisfiability"
weight: 32
pre: "2.3. "
---

We say that a logical statement is *satisfiable* when there exists at least one truth assignment that makes the overall statement true. 

In our Logika truth tables, this corresponds to statements that are either *contingent* or a *tautology*. (*Contradictory* statements are NOT satisfiable.)

For example, consider the following truth tables:

```text
          *
-----------------------
p q r | p → q V ¬r ∧ p
-----------------------
T T T |   T   T F  F
T T F |   T   T T  T
T F T |   F   F F  F
T F F |   T   T T  T
F T T |   T   T F  F
F T F |   T   T T  F
F F T |   T   F F  F
F F F |   T   F T  F
------------------------

Contingent

- T: [T T T] [T T F] [T F F] [F T T] [F T F] [F F T] [F F F]
- F: [T F T]
```

And

```text
      *
------------
p | p V ¬p 
------------
T |   T F
F |   T T
-------------

Tautology
```

Both of these statements are satisfiable, as they have at least one (or more than one) truth assignment that makes the overall statement true.