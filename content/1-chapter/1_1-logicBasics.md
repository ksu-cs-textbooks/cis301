---
title: "Basic Logical Reasoning"
weight: 20
pre: "1.1. "
---

## What is logical reasoning?

Logical reasoning is an analysis of an argument according to a set of rules. In this course, we will be learning several sets of rules for more formal analysis, but for now we will informally analyze English sentences and logic puzzles. This will help us practice the careful and rigorous thinking that we will need in formal proofs and in computer science in general.

## Premises and conclusions

A *premise* is a piece of information that we are given in a logical argument. In our reasoning, we assume premises are true -- even if they make no sense!

A *conclusion* in a logical argument is a statement whose validity we are checking. Sometimes we are given a conclusion, and we are trying to see whether that conclusion makes sense when we assume our premises are true. Other times, we are asked to come up with our own (valid) conclusion that we can deduce from our premises. 

## Example

Suppose we are given the following premises:

- Premise 1: *If a person wears a red shirt, then they don't like pizza.*
- Premise 2: *Fred is wearing a red shirt.*

Given those pieces of information, can we conclude the following?

*Fred doesn't like pizza.*


Yes! We take the premises at face value and assume them to be true (even though it is kind of ridiculous that shirt color has anything to do with a dislike of pizza). The first premise PROMISES that any time we have a person with a red shirt, then that person does not like pizza. Since Fred is such a person, we can conclude that Fred doesn't like pizza.

## Logical arguments with "OR"

Interpreting English sentences that use the word "or" can be tricky -- the or can either be an *inclusive or* or an *exclusive or*. In programming, we are used to using an inclusive or -- a statement like `p || q` is true as long as at least one of `p` or `q` is true, even if both are true. The only time a statement like `p || q` is false is if both `p` and `q` are false.

In English, however, the word "or" often implies an exclusive or. If a restaurant advertises that customers can choose "chips or fries" as the side for their meal, they are certainly not intending that a customer demand both sides. 

<b>However, since this course is focused on formal logic and analyzing computer programs and not so much on resolving language ambiguity, we will adopt the stance that the word "or" always means *inclusive or* unless otherwise specified.</b>

### Or example #1

With that in mind, suppose we have the following premises:

- *I have a dog or I have a cat.*
- *I do not have a cat.*

What can we conclude? 

The only time an "or" statement true is when at least one of its parts is true. Since we already know that the right side of the or ("I have a cat") is false, then we can conclude that the left side MUST be true. So we conclude:

*I have a dog.*

In general, if you have an or statement as a premise and you also know that one side of the or is NOT true, then you can always conclude that the other side of the or IS true.

### Or example #2

Suppose we have the following premises:

- *I have a bike or I have a car.*
- *I have a bike.*

Can we conclude anything new?

First of all, I acknowledge that the most natural interpretation of the first premise is an exclusive or -- that I have EITHER a bike OR a car, but not both. I think that is how most people would naturally interpret that sentence as well. However, in this course we will always consider "or" to be an inclusive or, unless we specifically use words like "but not both".

With that in mind, the second premise is already sufficient to make the first premise true. Since I have a bike, the statement "I have a bike or I have a car" is already true, whether or not I have a car. Because of this, we can't draw any further conclusions beyond our premises.

## Or example 3

Suppose we have the following premises:

- *I either have a bike or a car, but not both.*
- *I have a bike.*

What can we conclude?

This is the sentence structure I will use if I mean an exclusive or -- "either p or q but not both".

In this setup, we CAN conclude that I do not have a car. This is because an exclusive or is FALSE when both sides are true, and I already know that one side is true (I have a bike). The only way for the first premise to be true is when I do not also have a car.

## Logical arguments with *if/then* (aka *implies*, →)

Statements with of the form, *if p, then q* are making a promise -- that if *p* is true, then they promise that *q* will also be true. We will later see this structure using the logical *implies* operator.

### If/then example 1

Suppose we have the following premises:

- *If it is raining, then I will get wet.*
- *It is raining.*

What can I conclude?

The first premises PROMISES that if it is raining, then I will get wet. Since we assume this premise is true, then we must keep the promise. Since the second premise tells us that it IS raining, then we can conclude:

*I will get wet.*

### If/then example 2

Suppose we have the following premises:

- *If I don't hear my alarm, then I will be late for class.*
- *I am late for class.*

Can we conclude anything new?

The first premise promises that if I don't hear my alarm, then I will be late for class. And if we knew that I didn't hear my alarm, then we would be able to conclude that I will be late for class (in order to keep the promise).

However, we do NOT know that I don't hear my alarm. All we are told is that I am late for class. I might be late for class for many reasons -- maybe I got stuck in traffic, or my car broke down, or I got caught up playing a video game. We don't have enough information to conclude WHY I'm late for class, and in fact we can't conclude anything new at at all.

### If/then example 3

Suppose we have the following premises:

- *If I don't hear my alarm, then I will be late for class.*
- *I'm not late for class.*

What can we conclude?

This is a trickier example. We saw previously that the first premise promised that anytime I didn't hear my alarm, then I would be late for class. But we can interpret this another way -- since I'm NOT late for class, then I must have heard my alarm. After all, if I DIDN'T hear my alarm, then I would have been late. But I'm not late, so the opposite must be true. So we can conclude that:

*I hear my alarm.*

Reframing an if/then statement like that is called writing its *contrapositive*. Any time we have a statement of the form *if p, then q* then we can write the equivalent statement *if not q, then not p*.
