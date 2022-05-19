---
title: "Semantic Entailment"
weight: 34
pre: "2.5. "
---

## Definition

We say a set of premises, `p1`, `p2`, ..., `pn` semantically entail a conclusion `c`, and we write:

```text
p1, p2, ..., pn ⊨ c
```

if whenever we have a truth assignment that makes `p1`, `p2`, ..., `pn` all true, then `c` is also true for that truth assignment.

(Note: we can use the ASCII replacement `|=` instead of the Unicode `⊨`, if we want.)

## Showing semantic entailment

Suppose we have premises `p ^ q` and `p -> r`. We want to see if these premises necessarily entail the conclusion `r ^ q`.

First, we could make truth tables for each premise (being sure to list the variables `p`, `q` and `r` in each case, as that is the overall set of variables in the problem):

```text
//truth table for premise, p ^ q

          *
----------------------
p q r | p ^ q
----------------------
T T T |   T
T T F |   T
T F T |   F
T F F |   F
F T T |   F
F T F |   F
F F T |   F
F F F |   F
----------------------
Contingent

- T: [T T T] [T T F]
- F: [T F T] [T F F] [F T T] [F T F] [F F T] [F F F]
```

```text
//truth table for premise, p -> r

          *
----------------------
p q r | p -> r
----------------------
T T T |   T
T T F |   F
T F T |   T
T F F |   F
F T T |   T
F T F |   T
F F T |   T
F F F |   T
----------------------
Contingent

- T: [T T T] [T F T] [F T T] [F T F] [F F T] [F F F]
- F: [T T F] [T F F]
```

Now, we notice that the truth assignment `[T T T]` is the only one that makes both premises true. Next, we make a truth table for our potential conclusion, `r ^ q` (again, being sure to include all variables used in the problem):

```text
//truth table for potential conclusion, r ^ q
          *
----------------------
p q r | r ^ q
----------------------
T T T |   T
T T F |   F
T F T |   F
T F F |   F
F T T |   T
F T F |   F
F F T |   F
F F F |   F
----------------------
Contingent

- T: [T T T] [F T T]
- F: [T T F] [T F T] [T F F] [F T F] [F F T] [F F F]
```

Here, we notice that the truth assignment `[T T T]` makes the conclusion true as well. So we see that whenever there is a truth assignment that makes all of our premises true, then that same truth assignment also makes our conclusion true.

Thus, `p ^ q` and `p -> r` semantically entail the conclusion `r ^ q`, and we can write:

```text
p ^ q, p -> r ⊨ r ^ q
```

## Semantic entailment with one truth table

The process of making separate truth tables for each premise and the conclusion, and then examining each one to see if any truth assignment that makes all the premises true also makes the conclusion true, is fairly tedious. 

We are trying to show that IF each premise is true, THEN we promise the conclusion is true. This sounds exactly like an IMPLIES statement, and in fact that is what we can use to simplify our process. If we are trying to show that  `p1`, `p2`, ..., `pn` semantically entail a conclusion `c` (i.e., that `p1, p2, ..., pn ⊨ c`), then we can instead create ONE truth table for the statement:

`(p1 ^ p2 ^ ... ^ pn) -> c`

If this statement is a tautology (which would mean that anytime all the premises were true, then the conclusion was also true), then we would also have that the premises semantically entail the conclusion.

In our previous example, we create a truth table for the statement `(p ^ q) ^ (p -> r) -> r ^ q`:

```text
                           *
--------------------------------------
p q r | (p ^ q) ^ (p -> r) -> r ^ q
--------------------------------------
T T T |    T    T    T     T    T
T T F |    T    F    F     T    F
T F T |    F    F    T     T    F
T F F |    F    F    F     T    F
F T T |    F    F    T     T    T 
F T F |    F    F    T     T    F
F F T |    F    F    T     T    F
F F F |    F    F    T     T    F
---------------------------------------
Tautology
```

Then we see that it is indeed a tautology.
