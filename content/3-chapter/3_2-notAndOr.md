---
title: "NOT, AND, OR Translations"
pre: "3.2. "
weight: 41
date: 2018-08-24T10:53:26-05:00
---

Now that we have seen how to identify propositional atoms in English sentences, we will learn how to connect these propositions with logical operators in order to complete the process of translating from English to propositional logic.

## NOT translations
When you see the word "not" and the prefixes "un-" and "ir-", those should be replaced with a NOT operator. 

### Example 1

For example, if we have the sentence:

```text
I am not going to work today.
```

Then we would first identify the propositional atom:

```text
p: I am going to work today
```

and would then use a NOT operator to express the negation. Our full translation to propositional logic would be: ` ¬p`

### Example 2

As another example, suppose we have the sentence
```text
My sweater is irreplaceable.
```

We would identify the propositional atom:

```text
p: My sweater is replaceable.
```

And again, our complete translation would be: ` ¬p`

## AND translations

When you see the words "and", "but", "however", "moreover", "nevertheless", etc., then the English sentence is expressing a conjunction of ideas. When translating to propositional logic, all of these words should be replaced with a logical AND operator. 

It might seem strange that the sentences "It is cold and it is sunny" and "It is cold but it is sunny" should be translated the same way -- but really, both sentences are expressing two facts: 

1) It is cold
2) It is sunny

Using "but" instead of "and" in English adds a subtle comparison of the first fact to the second fact, but such nuances are beyond the capabilities of propositional logic (and are somewhat ambiguous anyway).

### Example 1

Suppose we want to translate the following sentence to propositional logic:

```text
I like cake but I don't like cupcakes.
```

We would first identify two propositional atoms:

```text
p: I like cake
q: I like cupcakes
```

We would then translate the clause "I don't like cupcakes" to ` ¬q`, and then would translate the connective "but" to a logical AND operator. We would finish with the following translation:

```text
p ∧  ¬q
```

### Example 2

Suppose we want to translate the following sentence to propositional logic:

```text
The school doesn't have both a pool and a track.
```

We would first identify two propositional atoms:

```text
p: The school has a pool
q: The school has a track
```

We would then see that we are really taking the sentence, "The school has a pool and a track" and negating it, which leaves us with the following translation: 

```text
 ¬(p ∧ q)
```

## OR translations

When you see the word "or" in a sentence, or some other clear disjunction of statements, then you will translate it to a logical OR operator. Because the word "or" in English can be ambiguous, We first need to determine whether the "or" is *inclusive* (in which case we would replace it with a regular OR operator) or *exclusive* (in which case we need to add a clause to explicitly express that both statements cannot be true).

As we saw in [section 1.1]({{<ref "1-chapter/1_1-logicBasics.md" >}}), the word "or" in an English sentence is usually meant to be exclusive. However, because the logical OR is *INclusive*, and since the purpose of this class is not to have you wrestle with subtleties of the English language, then you can assume that an "or" in a sentence is *inclusive* unless clearly stated otherwise.

### Inclusive OR statements

Suppose we want to translate the following sentence to propositional logic:

```text
You watch a movie and/or eat a snack.
```

We would first identify two propositional atoms:

```text
p: You watch a movie
q: You eat a snack
```

The "and/or" in our sentence makes it extremely clear that the intent is an inclusive or, since the sentence is true if you both watch a movie and eat a snack. This leaves us with the following translation: 

```text
p V q
```

### Exclusive OR statements

In this class, if the meaning of "or" in a sentence is meant to be exclusive, then the sentence will clearly state that the two statements aren't both true.

For example, suppose we want to translate the following sentence to propositional logic:

```text
On Saturday, Jane goes for a run or plays basketball, but not both.
```

We would first identify two propositional atoms:

```text
p: Jane goes for a run on Saturday
q: Jane plays basketball on Saturday
```

We then apply our equivalence for simulating an exclusive or operator, which we saw in [section 2.4]({{<ref "2-chapter/2_4-logicalEquiv.md" >}}). This leaves us with the following translation: 

```text
(p V q) ∧  ¬(p ∧ q)
```