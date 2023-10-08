---
title: "Single Quantifier"
pre: "5.3. "
weight: 62
date: 2018-08-24T10:53:26-05:00
---

In this section, we will see how to translate simpler statements between English to predicate logic. These translations will involve a single quantifier.

## Example: Predicate logic to English

Suppose our domain is animals and that we have the following two predicates:

- `isMouse(x)`: whether animal `x` is a mouse
- `inHouse(x)`: whether animal `x` is in the house

Suppose we also have that `Squeaky` is an individual in our domain.

We will practice translating from predicate logic to English. Think about what the following propositions mean, and click to reveal each answer:

- `isMouse(Squeaky) ∧ ¬inHouse(Squeaky)`

    - <details>
        <summary> <b> Click here for solution </b></summary>
            --> "Squeaky is a mouse, and Squeaky is not in the house."
        </details>
        <br>

- `∃ x isMouse(x)`

    - <details>
        <summary> <b> Click here for solution </b></summary>
            --> "There is a mouse".
        </details>
        <br>

- `¬(∃ x isMouse(x))`

    - <details>
        <summary> <b> Click here for solution </b></summary>
            --> "There is not a mouse."
        </details>
        <br>

- `∃ x ¬isMouse(x)`

    - <details>
        <summary> <b> Click here for solution </b></summary>
            --> "There is an animal that is not a mouse".
        </details>
        <br>

- `∀ x isMouse(x)`

    - <details>
        <summary> <b> Click here for solution </b></summary>
            --> "All animals are mice."
        </details>
        <br>

- `¬(∀ x isMouse(x))`

    - <details>
        <summary> <b> Click here for solution </b></summary>
            --> "Not all animals are mice."
        </details>
        <br>

- `∀ x ¬isMouse(x)`

    - <details>
        <summary> <b> Click here for solution </b></summary>
            --> "All animals are not mice."
        </details>
        <br>

- `∀ x (isMouse(x) → inHouse(x))`

    - <details>
        <summary> <b> Click here for solution </b></summary>
            --> "All mice are in the house."
        </details>
        <br>

- `∀ x (isMouse(x) ∧ inHouse(x))`

    - <details>
        <summary> <b> Click here for solution </b></summary>
            --> "Every animal is a mouse and is in the house." (We usually don't want ∧ with ∀.)
        </details>
        <br>

- `¬(∀ x (isMouse(x) → inHouse(x)))`

    - <details>
        <summary> <b> Click here for solution </b></summary>
            --> "Not all mice are in the house."
        </details>
        <br>

- `∀ x (inHouse(x) → isMouse(x))`
    - <details>
        <summary> <b> Click here for solution </b></summary>
            --> "Everything in the house is a mouse."
        </details>
        <br>

- `¬(∀ x (inHouse(x) → isMouse(x)))`

    - <details>
        <summary> <b> Click here for solution </b></summary>
            --> "Not everything in the house is a mouse."
        </details>
        <br>

- `∃ x (isMouse(x) ∧ inHouse(x))`

    - <details>
        <summary> <b> Click here for solution </b></summary>
            --> "There is a mouse in the house."
        </details>
        <br>

- `∃ x (isMouse(x) → inHouse(x))`

    - <details>
        <summary> <b> Click here for solution </b></summary>
            --> "There exists an animal, and if that animal is a mouse, then it is in the house." Recall that this statement will be true if there is an animal that is NOT a mouse (since the → would be vacuously true) as well as being true if there is a mouse in the house.
        </details>
        <br>

- `¬(∃ x (isMouse(x) ∧ inHouse(x)))`

    - <details>
        <summary> <b> Click here for solution </b></summary>
            --> "There is not a mouse in the house."
        </details>
        <br>

## Translation guide

When translating from English to predicate logic, you can look for particular wording in your sentences to see how to choose a quantifier and/or negation placement. We will also see that certain phrases can be translated multiple (equivalent) ways.

- *Every/all/each/any* is translated as: `∀ x ...`

- *Some/at least one/there exists/there is* is translated as: `∃ x ...`

- *None/no/there does not exist* can be translated as either `¬(∃ x ...)` or `∀ x ¬(...)`

- *Not every/not all* can be translated as either `¬(∀ x ...)` or `∃ x ¬(...)`

- *Some P-ish thing is a Q-ish thing* is translated as: `∃ x (P(x) ∧ Q(x))`

- *All P-ish things are Q-ish things* is translated as: `∀ x (P(x) → Q(x))`

- *No P-ish thing is a Q-ish thing* can be translated as either `¬(∃ x (P(x) ∧ Q(x)))` or `∀ x (P(x) → ¬Q(x))`

- *Not all P-ish things are Q-ish things* can be translated as either `¬(∀ x (P(x) → Q(x)))` or `∃ x (P(x) ∧ ¬Q(x))`

## DeMorgan's laws for quantifiers

In the translation guide above, we saw that we could often translate the same statement two different ways -- one way using an existential quantifier and one way using a universal quantifier. These equivalencies are another iteration of DeMorgan's laws, this time applied to predicate logic. 

Suppose we have some domain, and that `P(x)` is a predicate for individuals in that domain. DeMorgan's laws give us the following equivalencies:

- `¬(∃ x P(x))` is equivalent to `∀ x ¬P(x)`
- `¬(∀ x P(x))` is equivalent to `∃ x ¬P(x)`

In Chapter 6, we will learn to prove that these translations are indeed equivalent.

## Example: English to predicate logic

Suppose our domain is people and that we have the following two predicates:

- `K(x)`: whether person `x` is a kid
- `M(x)`: whether person `x` likes marshmallows

We will practice translating from English to predicate logic. Think about what the following sentences mean, and click to reveal each answer:

- *No kids like marshmallows.*
    - <details>
        <summary> <b> Click here for solution </b></summary>

        `¬(∃ x (K(x) ∧ M(x))`, or equivalently, `∀ x (K(x) → ¬M(x))`

        </details>
        <br>
- *Not all kids like marshmallows.*
    - <details>
        <summary> <b> Click here for solution </b></summary>

        `¬(∀ x (K(x) → M(x))`, or equivalently, `∃ x (K(x) ∧ ¬M(x))`

        </details>
        <br>
- *Everyone who likes marshmallows is a kid.*
    - <details>
        <summary> <b> Click here for solution </b></summary>

        `∀ x (M(x) → K(x))`

        </details>
        <br>
- *Some people who like marshmallows are not kids.*
    - <details>
        <summary> <b> Click here for solution </b></summary>

        `∃ x (M(x) ∧ ¬K(x))`

        </details>
        <br>
- *Some kids don't like marshmallows.*
    - <details>
        <summary> <b> Click here for solution </b></summary>

        `∃ x (K(x) ∧ ¬M(x))`

        </details>
        <br>
- *Anyone who doesn't like marshmallows is not a kid.*
    - <details>
        <summary> <b> Click here for solution </b></summary>

        `∀ x (¬M(x) → ¬K(x))`

        </details>
        <br>

## Evaluating predicate logic statements on a toy domain

Suppose we have the following toy domain of people with the following characteristics:

- Bob, age 10, lives in Kansas, has siblings, has brown hair
- Jane, age 25, lives in Delaware, has no siblings, has blonde hair
- Alice, age 66, lives in Kansas, has siblings, has gray hair
- Joe, age 50, lives in Nebraska, has siblings, has black hair

Now suppose that we have the following predicates for individuals in our domain:

- `Ad(x)`: whether person `x` is an adult (adults are age 18 and older)
- `KS(x)`: whether person `x` lives in Kansas
- `Sib(x)`: whether person `x` has siblings
- `Red(x)`: whether person `x` has red hair

We will practice evaluating predicate logic statements on our domain of people. Think about whether the following propositions would be true or false over our domain, and then click to reveal each answer:

- `∀ x Ad(x)`
    - <details>
        <summary> <b> Click here for solution </b></summary>
            This proposition translates as, "All people are adults". This is false for our domain, as we have one person (Bob) who is not an adult.
        </details>
        <br>

 - `∀ x ¬Ad(x)`
    - <details>
        <summary> <b> Click here for solution </b></summary>
            This proposition translates as, "All people are not adults". This is false for our domain, as we have three people (Jane, Alice, and Joe) are are adults.
        </details>
        <br>       

 - `¬(∀ x Ad(x))`
    - <details>
        <summary> <b> Click here for solution </b></summary>
            This proposition translates as, "Not all people are adults". This is true for our domain, as we can find a person (Bob) who is not an adult.
        </details>
        <br>       

 - `∀ x (KS(x) → Sib(x))`
    - <details>
        <summary> <b> Click here for solution </b></summary>
            This proposition translates as, "Everyone who lives in Kansas has siblings". This is true for our domain, as we have two people who live in Kansas (Bob and Alice), and both of them have siblings.
        </details>
        <br>

 - `∃ x (¬KS(x) ∧ Sib(x))`
    - <details>
        <summary> <b> Click here for solution </b></summary>
            This proposition translates as, "There is a person who doesn't live in Kansas and has siblings". This is true for our domain, as Joe lives in Nebraska and has siblings.
        </details>
        <br>  

 - `¬(∃ x (KS(x) ∧ ¬Ad(x))`
    - <details>
        <summary> <b> Click here for solution </b></summary>
            This proposition translates as, "There does not exist a person who lives in Kansas and is not an adult". This is false for our domain, as Bob lives in Kansas and is not an adult.
        <br> 

 - `¬(∃ x (Sib(x) ∧ Red(x))`
    - <details>
        <summary> <b> Click here for solution </b></summary>
            This proposition translates as, "There does not exist a person with siblings who has red hair". This is true for our domain, as no one with siblings (Bob, Alice, or Joe) has red hair.
        <br>

 - `∀ x (Red(x) → Sib(x))`
    - <details>
        <summary> <b> Click here for solution </b></summary>
            This proposition translates as, "All people with red hair have siblings". This is true for our domain, as no one has red hair. This means that the implies statement is vacuously true for every person (since `Red(x)` is false for each person), which makes the overall proposition true.
        <br> 

 - `∀ x (KS(x) ∨ Sib(x))`
    - <details>
        <summary> <b> Click here for solution </b></summary>
            This proposition translates as, "Everyone lives in Kansas and/or has siblings". This is false for our domain -- there is one person, Jane, who doesn't live in Kansas and also doesn't have siblings.
        <br> 