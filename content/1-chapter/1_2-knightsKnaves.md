---
title: "Knights and Knaves"
weight: 21
pre: "1.2. "
---

We will now move to solving several kinds of logic puzzles. While these puzzles aren't strictly necessary to understand the remaining course content, they require the same rigorous analysis that we will use when doing more formal truth tables and proofs. Plus, they're fun!

The puzzles in this section and the rest of this chapter are all either from or inspired by: *What is the Name of This Book?*, by Raymond Smullyan.

## Island of Knights and Knaves

This section will involve knights and knaves puzzles, where we meet different inhabitants of the mythical island of Knights and Knaves. Each inhabitant of this island is either a *knight* or a *knave*.

Knights ALWAYS tell the truth, and knaves ALWAYS lie.

## Example 1

Can any inhabitant of the island of Knights and Knaves say, "I'm a knave"?

<details>
    <summary> <b> --> Click for solution </b></summary>


No! A knight couldn't make that statement, as knights always tell the truth. And a knave couldn't make that statement either, since it would be true -- and knaves always lie.


</details>

## Example 2

You see two inhabitants of the island of Knights and Knaves -- Ava and Bob.

- Ava says that Bob is a knave.
- Bob says, "Neither Ava nor I are knaves."

What types are Ava and Bob?

<details>
    <summary> <b> --> Click for solution </b></summary>

Suppose Ava is a knight. Then her statement must be true, so Bob must be a knave. In this case, Bob's statement would be a lie (since he is a knave), which is what we want.

Let's make sure there aren't any other answers that work.

Suppose instead that Ava is a knave. Then her statement must be a lie, so Bob must be a knight. This would mean that Bob's statement should be true, but it's not -- Ava *is* a knave.

We can conclude that Ava is a knight and Bob is a knave.

</details>

## Example 3

If you see an "or" statement in a knights and knaves puzzle, assume that it means an *inclusive* or. This will match the *or* logical operator in our later truth tables and proofs, and will also match the or operator in programmimg.

You see two different inhabitants -- Eve and Fred.

- Eve says, "I am a knave or Fred is a knight."

What types are Eve and Fred?

<details>
    <summary> <b> --> Click for solution </b></summary>

Suppose first that Eve is a knight. Then her statement must be true. Since she isn't a knave, the only way for her statement to be true is if Fred is a knight.

Let's make sure there aren't any other answers that work.

Suppose instead that Eve is a knave. Already we are in trouble -- Eve's statement is already true no matter what type Fred is. Since Eve would lie if she was a knave, we know she must not be knave.

We can conclude that Eve and Fred are both knights.

</details>

## Example 4

You see three new inhabitants -- Sarah, Bill, and Mae.

- Sarah tells you that only a knave would say that Bill is a knave.
- Bill claims that it's false that Mae is a knave.
- Mae tells you, "Bill would tell you that I am a knave."

What types are Sarah, Bill, and Mae?

<details>
    <summary> <b> --> Click for solution </b></summary>

Before starting on this puzzle, it might help to rephrase Sarah's and Bill's statements. Sarah's statement that only a knave would say that Bill is knave is really saying that it is FALSE that Bill is a knave (since knaves lie). Another way to say it's false that Bill is a knave is to say that Bill is a knight. Similarly, we can rewrite Bill's statemnet to say that Mae is a knight.

Now we have the following statements:

- Sarah tells you that Bill is a knight.
- Bill claims that Mae is a knight.
- Mae tells you, "Bill would tell you that I am a knave."

Suppose Sarah is a knight. Then her statement is true, so Bill must also be a knight. This would mean Bill's statement would also be true, so Mae is a knight as well. But Mae says that Bill would say she's a knave, and that's not true -- Bill would truthfully say that Mae is a knight.

Suppose instead that Sarah is a knave. Then her statement is false, so Bill must be a knave. This would make Bill's claim false as well, so Mae must be a knave. Mae knows that Bill would say she was a knight (since Bill is a knave, and would lie), and if Mae was a knave then she would indeed lie and say that Bill would say she was a knave.

We can conclude that all three are knaves.

</details>


