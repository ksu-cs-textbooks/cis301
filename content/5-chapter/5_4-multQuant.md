---
title: "Multiple Quantifiers"
pre: "5.4. "
weight: 63
date: 2018-08-24T10:53:26-05:00
---

Translations that involve more than one quantifier (which often happens when some of the predicates have more than one parameter) are more challenging. We will divide these translations into two categories:

- Translations that involve several of the same quantifier (multiple universal quantifiers or multiple existential quantifiiers)
- Translations that mix quantifiers

In many of the sections, we will using the predicates below (which are over the domain of shapes):

- `isCircle(x)` - whether shape x is a circle
- `isSquare(x)` - whether shape x is a square
- `isRectangle(x)` - whether shape x is a rectangle
- `biggerThan(x, y)` - whether shape x is bigger than shape y

## Several of the same quantifier

First, we consider translations that involve several of the same quantifier. There are two ways we can translates such statements -- either using prenex form (quantifiers out front) or Aristotlian form (quantifiers nested).

### Prenex form

The *prenex form* of a predicate logic translation lists all the quantifiers at the beginning of the statement. This is only recommended when all the quantifiers are the same type -- either all universal or all existential.

#### Prenex example 1

Suppose we wished to translate, *Some circle is bigger than some square*. Here, we are making three claims:

- There exists a shape that is a circle
- There exists a shape that is a square
- The shape that is a circle is bigger than the shape that is a square

With that in mind, we can see that we will use two existential quantifiers. We can translate the statement as follows:

```text
∃ x ∃ y (isCircle(x) ∧ isSquare(y) ∧ biggerThan(x, y))
```

Which reads: *There are two shapes, x and y, where x is a circle, y is a square, and x (which is a circle) is bigger than y (which is a square).*

Equivalently, we could have written:

```text
∃ x ∃ y (isCircle(y) ∧ isSquare(x) ∧ biggerThan(y, x))
```

Which reads: *There are two shapes, x and y, where y is a circle, x is a square, and y (which is a circle) is bigger than x (which is a square).*

#### Prenex example 2

Next, suppose we wished to translate: *Every circle is bigger than all rectangles*. Again, we are quantifying two things -- ALL circles and also ALL squares. We can see that we will need to use two universal quantifiers. We can translate the statement as follows:

```text
∀ x ∀ y ((isCircle(x) ∧ isSquare(y)) → biggerThan(x, y))
```

Which reads: *For each combination (x, y) of shapes, if x is a circle and y is a square, then x (which is a circle) is bigger than y (which is a square).*

### Aristotlian form

The *Aristotlian form* of a predicate logic translation embeds the quantifiers within the translation. This format is possible for any kind of translation -- whether the quantifiers are all the same type or mixed types.

#### Aristotlian form example 1

Suppose we wished to translate, *Some circle is bigger than some square* using Aristotlian form. We know that will will still need two existential quantifiers, but we will only introduce each quantifier just before the corresponding variable is needed in a predicate.

We can translate the statement using Aristotlian form as follows:

```text
∃ x (isCircle(x) ∧ (∃ y (isSquare(y) ∧ biggerThan(x, y)))
```

Which reads as: *There exists a shape x that is a circle and there exists a shape y that is a square, and x (which is a circle) is bigger than y (which is a square)*.

#### Aristotlian form example 2

Let's repeat our translation for, *Every circle is bigger than all rectangles* using Aristotlian form. We know that will will still need two existential quantifiers, but we will only introduce each quantifier just before the corresponding variable is needed in a predicate.

We can translate the statement using Aristotlian form as follows:

```text
∀ x (isCircle(x) → (∀ y (isSquare(y) → biggerThan(x, y))))
```

Which reads as: *For every shape x, if x is a circle, then for every shape y, if y is s square, then x (which is a circle) is bigger than y (which is a square).*

## Mixed quantifiers

Now, we will turn to examples that mix universal and existential quantifiers. We will see below that quantifier order matters in this case, so it is safest to translate such statements using embedded quantifiiers. The embedded form can be tricky to write, so we will see a way to systematically translated any statement that needs multiple quantifiers into predicate logic (using Aristotlian form).

### Systematic translation

Suppose we wish to translate, *Every circle is bigger than at least one square*. We see that we are first making a claim about all circles. Without worrying about the rest of the statement, we know that for all circles, we are saying *something*. So we write:

```text
For all circles, SOMETHING
```

Trying to formalize a bit more, we assign a variable to the current circle we are describing (`x`). For each circle x, we are saying *something* about that circle. So we express SOMETHING(x) as some claim about our current circle, and write:

```text
For each circle x, SOMETHING(x)
```

We see that we will need a universal quantifier since we are talking about ALL circles, and we also follow the guide of using an implies statement to work with a for-all statement:

```text
∀ x (isCircle(x) → SOMETHING(x))
```

Next, we describe what `SOMETHING(x)` means for a particular circle, `x`:

```text
SOMETHING(x): x is bigger than at least one square
```

Trying to formalize a bit more about the square, we write:

```text
SOMETHING(x): There exists a square y, and x is bigger than y
```

Now we can use an existential quantifier to describe our square, and plug in our isSquare and biggerThan predicates to have a translation for SOMETHING(x):

```text
SOMETHING(x): ∃ y (isSquare(y) ∧ biggerThan(x, y))
```

Now, we can plug SOMETHING(x) into our first partial translation, `∀ x (isCircle(x) → SOMETHING(x))`. The complete translation for *Every circle is bigger than at least one square* is:

```text
∀ x (isCircle(x) → (∃ y (isSquare(y) ∧ biggerThan(x, y))))
```

### Follow-up examples

In these examples, suppose our domain is animals and that we have the following predicates:

- `El(x)`: whether animal x is an elephant
- `Hi(x)`: whether animal x is a hippo
- `W(x, y)`: whether animal x weighs more than animal y

Suppose we wish to translate: *There is exactly one hippo.* We might first try saying: `∃ x Hi(x)`. But this proposition would be true even if we had 100 hippos, so we need something more restricted. What we are really trying to say is:

- There exists a hippo
- AND, any other hippo is the same one

Let's use our systematic approach, streamlining a few of the steps:

- There exists an animal x that is a hippo, and SOMETHING(x)
- `∃ x (Hi(x) ∧ SOMETHING(x))`

To translate SOMETHING(x), the claim we are making about our hippo x:

- `SOMETHING(x)`: any other hippo is the same as x
- `SOMETHING(x)`: for each hippo y, x is the same as y
- `SOMETHING(x)`: `∀ y (Hi(y) → (x == y))

Now we can put everything together to get a complete translation:

```text
∃ x (Hi(x) ∧ (∀ y (Hi(y) → (x == y)))
```

Here are a few more translations from English to predicate logic. Think about what the following statements mean, and click to reveal each answer:

- *Every elephant is heavier than some hippo.*
    - <details>
        <summary> <b> Click here for solution </b></summary>

        `∀ x (El(x) -> (∃ y (Hi(y) ^ W(x, y))))`

        </details>
        <br>

- *There is an elephant that is heavier than all hippos.*
    - <details>
        <summary> <b> Click here for solution </b></summary>

        `∃ x (El(x) ^ (∀ y (Hi(y) -> W(x, y))))`

        </details>
        <br>

- *No hippo is heavier than every elephant.*
    - <details>
        <summary> <b> Click here for solution </b></summary>

        `¬(∃ x (Hi(x) ^ (∀ y (El(y) -> W(x, y)))))`

        </details>
        <br>

### Order matters!

We have learned that when dealing with mixed quantifiers, it is safest to embed them within the translation. If we put the mixed quantifiers out front of a translation, then we can accidentally include them in the wrong order and end up with an incorrect translation.

Suppose we have this predicate, over the domain of people:

```text
likes(x, y): whether person x likes person y
```

Further suppose that liking a person is not necessarily symmetric: that just because person x likes person y does not mean that person y necessarily likes person x. 

Consider these pairs of propositions:

```text 
∀ x ∀ y likes(x, y)     vs.     ∀ y ∀ x likes(x, y)  
∃ x ∃ y likes(x, y)     vs.     ∃ y ∃ x likes(x, y)
```

Is there any difference between each one? No! The two versions of the first proposition both say that every person likes every other person, and the two versions of the second proposition both say that there is a person who likes another person.

But what about:

```text 
∀ x ∃ y likes(x, y)     vs.     ∃ y ∀ x likes(x, y)
```

Suppose our domain is made up of the following people:

```text
Bob: likes Alice and James
Alice: likes Bob
James: likes Alice
```

The first proposition, `∀ x ∃ y likes(x, y)`, says that all people have some person (not necessarily the same person) that they like. This would certainly be true for our domain, as every person has at least one person that they like. The second proposition, `∃ y ∀ x likes(x, y)` is saying that there is a person (the SAME person) that everyone likes. This proposition would be false for our domain, as there is no one person that is liked by everyone.

## Precedence with quantifiers

In section 2.2, we discussed operator precedence for propositional logic statements. The same operator precedence holds for predicate logic statements, except that our two quantifiers (`∀` and `∃`) have the same precedence as the NOT operator. If we have a proposition with multiple quantifiers, then the quantifiers are resolved from right to left. For example, `∃ y ∀ x likes(x, y)` should be interpreted as `∃ y (∀ x likes(x, y))`.

Here is an updated list of operator precedence, from most important (do first) to least important (do last):

1) Parentheses
2) Not operator (`¬`), universal quantifier (`∀`), existential quantifier (`∃`)
3) And operator, `∧`
4) Or operator, `∨`
5) Implies operator, `→`

And here is our updated list of how to resolve multiple operators with the same precedence:

1) Multiple parentheses - the innermost parentheses are resolved first, working from inside out.
2) Multiple not ( `¬` ) operators -- the rightmost `¬` is resolved first, working from right to left. For example, `¬¬p` is equivalent to `¬(¬p)`.
3) Multiple and ( `∧` ) operators -- the leftmost `∧` is resolved first, working from left to right. For example, `p ∧ q ∧ r` is equivalent to `(p ∧ q) ∧ r`.
4) Multiple or ( `∨` ) operators -- the leftmost `∨` is resolved first, working from left to right. For example, `p ∨ q ∨ r` is equivalent to `(p ∨ q) ∨ r`.
5) Multiple implies ( `→` ) operators -- the rightmost `→` is resolved first, working from right to left. For example, `p → q → r` is equivalent to `p → (q → r)`.
6) Multiple quantifiers -- the rightmost quantifier is resolved first, working from right to left. For example, `∃ y ∀ x likes(x, y)` should be interpreted as `∃ y (∀ x likes(x, y))`.

When we get to predicate logic proofs in Chapter 6, we will see that Logika uses a different precedence for quantifiers -- there, quantifiers have the LOWEST precedence (done last) of any operator. This ends up being more forgiving than confusing, as it will accept propositions as correct that are really missing parentheses. For example, Logika will accept: `∃ x isMouse(x) ∧ inHouse(x)`. Technically, this proposition *should* be incorrect -- if we correctly treat quantifiers as having a higher precedence than `∧`, then `inHouse(x)` would no longer know about the variable `x` or its quantifier. 

We *should* use parentheses with quantifiers to express our intended meaning, and so we should write `∃ x (isMouse(x) ∧ inHouse(x))` instead. But if we forget the parentheses, then Logika will forgive us.