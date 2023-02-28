---
title: "Syntax"
pre: "5.2. "
weight: 61
date: 2018-08-24T10:53:26-05:00
---

In this section, we will examine the syntax for translating English sentences to predicate logic. We will still create propositions (statements that are either true or false) using logical connectives (`∧`, `∨`, `→`, and `¬`), but now we will identify the following from our English sentences

```text
predicates: these will be the verbs in the sentences
individuals: these will be the nouns in the sentences
quantifiers: these will help us specify if we mean all individuals or at least one individual
```

## Domains
Predicate logic involves expressing truth about a set of individuals. But the same statement might be true for one group of individuals, but false for others. Thus, we first need to consider which set of individuals we are discussing -- called the **domain**.

A domain might be the set of all humans, the set of all animals, the set of all college classes, etc.

## Individuals

An **individual** is an element within a specified domain. For example, if our domain is the set of all people, then `Bob` might be a particular individual. If our domain is the set of all college classes, then `CIS301` might be a particular individual.

## Predicates

A **predicate** is a function that returns a boolean. It can have one or many parameters, each of which are individuals in a particular domain. A predicate will describe a characteristic of an individual or a comparison betwen multiple individuals.

For example, suppose our domain is the set of people. Suppose `Alice`, `Bob`, and `Carla` are individuals in our domain. `Alice` is `Bob`'s mother, and `Carla` is an unrelated individual. `Carla` is 5'10 and 20 years old, `Alice` is 5'5 and 35 years old, and `Bob` is 4'10 and 10 years old.

Suppose we have the predicates:

- `isAdult(person)` - returns whether `person` is an adult
- `isMotherOf(person1, person2)` - returns whether `person1` is the mother of `person2`
- `isTallerThan(person1, person2)` - returns whether `person1` is taller than `person2`

Using our individuals above, we would have that:

- `isAdult(Alice)` is true, since `Alice` is 35 years old
- `isAdult(Bob)` is false, since `Bob` is 10 years old
- `isMotherOf(Alice, Bob)` is true, since `Alice` is `Bob`'s mother
- `isMotherOf(Carla, Bob)` is false, since `Carla` is not `Bob`'s mother
- `isTallerThan(Carla, Alice)` is true, since `Carla` is 5'10 and `Alice` is 5'5.

## Quantifiers

We will introduce two **quantifiers** in predicate logic, which help us make claims about a domain of individuals.

### Universal quantifier

The `∀` quantifier, called the **universal quantifier** and read as *for all*, lets us write propositions that pertain to ALL individuals in a domain.

 `∀ n P(n)` means: for every individual `n` (in some domain), `P(n)` is true. Here, `n` is a variable that stands for a particular individual in the domain. You can think of it like a foreach loop in C#:

 ```text
 foreach(type n in domain)
 {
     //P(n) is true every time
 }
 ```

 where `n` is initially the first individual in the domain, then `n` is the second individual in the domain, etc. 

### Existential quantifier

 The `∃` quantifier, called the **existential quantifier** and read as *there exists*, lets us write propositions that pertain to AT LEAST ONE individual in a domain. 

`∃ n P(n)` means: there exists at least one individual `n` (in some domain) where `P(n)` is true. You can again think of it as a foreach loop:

 ```text
 foreach(type n in domain)
 {
     //we can find at least one time where P(n) is true
 }
 ```

### Universal quantifier example

For example, suppose our domain is all candy bars, and that we have the predicate `isSweet(bar)`, which returns whether `bar` is sweet. We might write:

```text
∀ x isSweet(x)
```

Which we would read as: *for all candy bars x, x is sweet*, or, more compactly, as: *all candy bars are sweet*.

### Existential quantifier example

If instead we wrote:

```text
∃ x isSweet(x)
```

We would read it as: *there exists at least one candy bar x where x is sweet*, or, more compactly, as *there exists at least one sweet candy bar*.

## Early examples

Suppose our domain is animals, and that we have the following two predicates:

- `isDog(x)`: whether animal x is a dog
- `hasFourLegs(x)`: whether animal x has four legs

Let's consider what several predicate logic statements would mean in words:

- `∀ x isDog(x)` - translates to: *All animals are dogs*. This means that EVERY SINGLE ANIMAL in my domain is a dog (which is probably unlikely).
- `∃ x hasFourLegs(x)` - translates to: *There exists at least one animal that has four legs.*

Next, consider the following proposition:

```text
∀ x (isDog(x) ∧ hasFourLegs(x))
```

This translates to: *All animals are dogs and have four legs*. This means that EVERY SINGLE ANIMAL in my domain is a dog and also has four legs. While it is possible that this is true depending on our domain, it is unlikely. What if our domain of animals included cats, chicken,, etc.?

Perhaps instead we intended to say: *All dogs have four legs.* Another way to phrase this is, "For all animals, IF that animal is a dog, THEN it has four legs." We can see from the IF...THEN that we will need to use an implies statement. Here is the correct translation for *All dogs have four legs*:

```text
∀ x (isDog(x) → hasFourLegs(x))
```

We will usually want to use the `→` operator instead of the `∧` operator when making a claim about ALL individuals.

Finally, consider this proposition: 

```text
∃ x (isDog(x) → hasFourLegs(x))
```

This translates to: *There exists an animal x, and if that animal is a dog, then it has four legs.* Recall that an implies statement `p→q` is true whenever `p` and `q` are both true AND whenever `p` is false. So this claim is true in two cases:

- If our domain includes a dog that has four legs
- If our domain includes an animal that is not a dog

We likely only meant to include the first case. In that case, we would want to say, *There exists a dog that has four legs* -- here is that translation:

```text
∃ x (isDog(x) ∧ hasFourLegs(x))
```

We will usually want to use the `∧` operator instead of the `→` operator when writing a proposition about one/some individuals.

## Predicates from math

All of our examples in this section involved predicates over domains like people, animals, or living things. A different domain that we are used to working with is some set of numbers: the integers, the positive numbers, the rational numbers, etc. 

Perhaps our domain is the set of all integers. Then `>` is a predicate with two parameters -- `x > y` is defined as whether `x` is bigger than `y`, for two integers `x` and `y`. We might write:

```text
∀ x (x + 1 > x)
```

Because for all integers, `x + 1` is bigger than `x`. We might also write:

```text
∃ x (x > x * x * x)
```

Because -4 > -4 * -4 * -4, i.e., -4 > -64. The same is true for any negative number.

Other common predicates in math are: `<`, `<=`, `>=`, `==`, and `!=`.

## Quantifier symbols

The official symbol for the universal quantifier ("for all") is an upside-down A, like this: `∀`. You are welcome to substitute either a capital `A`, or with the word `all` or `forall`. This will be especially handy when we reach Chapter 6 on writing proofs in predicate logic.

The official symbol for the existential quantifier ("there exists") is a backwards E, like this: `∃`. You are welcome to substitute either a capital `E`, or with the word `some` or `exists`.