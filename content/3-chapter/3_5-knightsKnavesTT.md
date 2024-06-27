---
title: "Knights and Knaves, revisited"
weight: 44
pre: "3.5. "
---

Recall the Knights and Knaves puzzles from [section 1.2]({{% ref "1-chapter/1_2-knightsKnaves.md"  %}}). In addition to solving these puzzle by hand, we can devise a strategy to first translate a Knights and Knaves puzzle to propositional logic, and then solve the puzzle using a truth table.

## Identifying propositional atoms

To translate a Knights and Knaves puzzle to propositional logic, we first create a propositional atom for each person that represented whether that person was a knight. For example, if our puzzle included the people "Adam", "Bob", and "Carly", then we might create propositional atoms `a`, `b`, and `c`:

```text
a: Adam is a knight
b: Bob is a knight
c: Carly is a knight
```

## Translating statements

Once we have our propositional atoms, we can translate each statement in the puzzle to propositional logic. For each one, we want to capture that the statement is true IF AND ONLY IF the person speaking is a knight. (That way, the statement would be false whenever the person was not a knight -- i.e., when they were a knave.) We recall that we can express *if and only if* using a conjunction of implications. So if we want to write `p if and only if q`, then we can say `(p → q) ∧ (q → p)`.

As an example, suppose we have the following statement:

```text
Adam says: Bob is a knight and Carly is a knave.
```

Adam's statement should be true if and only if he is a knight, so we can translate it as follows:

```text
(a → (b ∧  ¬c)) ∧ ((b ∧  ¬c) → a)
```

Which reads as:

```text
If I am a knight, then Bob is a knight and Carly is a knave. Also, if Bob is a knight and Carly is a knave, then I am a knight.
```

We repeat this process for each statement in the puzzle. Finally, since we solve a Knights and Knaves puzzle by finding a truth assignment (i.e., assignment of who is a knight and who is a knave) that works for ALL statements, then we finish by AND-ing together our translations for each speaker. When we fill in the truth table for our final combined proposition, then a valid solution to the puzzle is any truth assignment that makes the overall proposition true. If it was a well-made puzzle, then there should only be one such truth assignment.

## Full example

Suppose we meet two people on the Island of Knights and Knaves -- Ava and Bob.

```text
Ava says, "Bob and I are not the same".
Bob says, "Of Ava and I, exactly one is a knight."
```

We first create a propositional atom for each person:

```text
a: Ava is a knight
b: Bob is a knight
```

Then, we translate each statement:

- *Bob and I are not the same*
    - Translation: `(a → (a ∧ ¬b V ¬a ∧ b)) ∧ ((a ∧ ¬b V ¬a ∧ b) → a)`
    - Meaning: If Ava is a knight, then either Ava is a knight and Bob is a knave, or Ava is a knave and Bob is a knight (so they aren't the same type). Also, if Ava and Bob aren't the same type, then Ava must be a knight (because her statement would be true).
- *Bob says, "Of Ava and I, exactly one is a knight.*
    - Bob is really saying the same thing as Ava...if exactly one is a knight, then either Ava is a knight and Bob is a knave, or Ava is a knave and Bob is a knight.
    - Translation: `(b → (a ∧ ¬b V ¬a ∧ b)) ∧ ((a ∧ ¬b V ¬a ∧ b) → b)`

We combine our translations for Ava and Bob and end up with the following propositional logic statement:

```text
(a → (a ∧ ¬b V ¬a ∧ b)) ∧ ((a ∧ ¬b V ¬a ∧ b) → a) ∧ (b → (a ∧ ¬b V ¬a ∧ b)) ∧ ((a ∧ ¬b V ¬a ∧ b) → b)`
```

We then complete the truth table for that proposition:

```text
                                                                                  *
---------------------------------------------------------------------------------------------------------------
a b | (a →: (a ∧ ¬b V ¬a ∧ b)) ∧ ((a ∧ ¬b V ¬a ∧ b) →: a) ∧ (b →:(a ∧ ¬b V ¬a ∧ b)) ∧ ((a ∧ ¬b V ¬a ∧ b) →: b)
---------------------------------------------------------------------------------------------------------------
T T |    F     F F  F F  F     F     F F  F F  F    T     F    F    F  F F F  F     F     F  F F  F  F   T
T F |    T     T T  T F  F     T     T T  T F  F    T     T    T    T  T T F  F     F     T  T T  F  F   F
F T |    T     F F  T T  T     F     F F  T T  T    F     F    T    F  F T T  T     F     F  F T  T  T   T
F F |    T     F T  F T  F     T     F T  F T  F    T     T    T    F  T F T  F     T     F  T F  T  F   T
---------------------------------------------------------------------------------------------------------------
Contingent
T: [F F]
F: [T T] [T F] [F T]
```

And we see that there is only one truth assignment that satisfies the proposition -- `[F F]`, which corresponds to Ava being a knave and Bob being a knave.

## Conclusion

As you can see, solving a Knights and Knaves problem by translating each statement to propositional logic is a tedious process. We ended up with a very involved final formula that made filling in the truth table somewhat arduous. Such problems are usually much simpler to solve by hand -- but this process demonstrates that we *can* apply a systematic approach to solve Knights and Knaves problems with translations and truth tables.