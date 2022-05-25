---
title: "Implies Translations"
pre: "3.3. "
weight: 42
date: 2018-08-24T10:53:26-05:00
---

In this section, we will learn when to use an implies (→) operator when translating from English to propositional logic. In general, you will want to use an implies operator any time a sentence is making a promise -- if one thing happens, then we promise that another thing will happen. The trick is to figure out the direction of the promise -- promising that if p happens, then q will happen is subtly different from promising that if q happens, then p will happen.

Look for the words "if", "only if", "unless", "except", and "provided" as clues that the propositional logic translation will use an implies operator.

## IF p THEN q statements

An "IF p THEN q" statement is promising that if `p` is true, then we can infer that `q` is also true. (It is making NO claims about what we can infer if we know `q` is true.)

For example, consider the following sentence:

```text
If it is hot today, then I'll get ice cream.
```

We first identify the following propositional atoms:

```text
p: It is hot today
q: I'll get ice cream
```

To determine the order of the implication, we think about what is being promised -- if it is hot, then we can infer that ice cream will happen. But if we get ice cream, then we have no idea what the weather is like. It might be hot, but it might also be cold and I just decided to get ice cream anyway. Thus we finish with the following translation:

```text
p → q
```

Alternatively, if we don't get ice cream, we can be certain that it wasn't hot -- since we are promsied that if it is hot, then we will get ice cream. So an equivalent translation is:

```text
 ¬q →  ¬p
```

(This form is called the *contrapositive*, which we learned about it in [section 2.4]({{<ref "2-chapter/2_4-logicalEquiv.md" >}})). We'll study it and other equivalent translations more in the next section.)

## p IF q statements

A "p IF q" statement is promising that if `q` is true, then we can infer that `p` is also true. (It is making NO claims about what we can infer if we know `p` is true.) Equivalent ways of expressing the same promise are "p PROVIDED q" and "p WHEN q".

For example, consider the following sentence:

```text
You can login to a CS lab computer if you have a CS account.
```

We first identify the following propositional atoms:

```text
p: You can login to a CS lab computer
q: You have a CS account
```

To determine the order of the implication, we think about what conditions need to be met in order for me to be promised that I can login. We see that if we have a CS account, then we are promised to be able to login. Thus we finish with the following translation:

```text
q → p
```

In this example, if we knew we could login to a CS lab computer, we wouldn't be certain that we had a CS account. There might be other reasons we can login -- maybe you can use your eID account instead, for example.

Alternatively, if we can't login, we can be certain that we don't have a CS account. After all, we are guaranteed to be able to login if we do have a CS account. So another valid translation is:

```text
 ¬p →  ¬q
```

## p ONLY IF q

A "p ONLY IF q" statement is promising that the only time `p` happens is when `q` also happens. So if `p` does happen, it must be the case that `q` did too (since `p` can't happen without `q` happening too).

For example, consider the following sentence:

```text
Wilma eats cookies only if Evelyn makes them.
```

We first identify the following propositional atoms:

```text
p: Wilma eats cookies
q: Evelyn makes cookies
```

What conditions need to be met for Wilma to eat cookies? If Wilma is eating cookies, Evelyn must have made cookies -- after all, we know that Wilma only eats Evelyn's cookies. Thus we finish with the following translation:

```text
p → q
```

Equivalently, we are certain that if Evelyn DOESN'T make cookies, then Wilma won't eat them -- since she only eats Evelyn's cookies. We can also write:

```text
 ¬q →  ¬p
```

However, if we know that Evelyn makes cookies, we can't be sure that Wilma will eat them. We know she won't eat any other kind of cookie, but maybe Wilma is full today and won't even eat Evelyn's cookies...we can't be sure.


## p UNLESS b, p EXCEPT IF q

The statements "p UNLESS q" and "p EXCEPT IF q" are equivalent...and both can sometimes be ambiguous. For example, consider the following sentence:

```text
I will bike to work unless it is raining.
```

We first identify the following propositional atoms:

```text
p: I will bike to work
q: It is raining
```

Next, we consider what exactly is being promised:

- *If it isn't raining, will I bike to work?* &nbsp; YES ¬ I promise to bike to work whenever it isn't raining.
- *If I don't bike to work, is it raining?* &nbsp; YES ¬ I always bike when it's not raining, so if I don't bike, it must be raining.
- *If it's raining, will I necessarily not bike to work?* &nbsp; Well, maybe? Some people might interpret the sentence as saying I'll get to work in another way if it's raining, and others might think no promise has been made if it is raining.
- *If I bike to work, is it necessarily not raining?* &nbsp; Again, maybe? It's not clear if I'm promising to only bike in non-rainy weather.

As we did with ambiguous OR statements, we will establish a rule in this class for intrepeting "unless" statements that will let us resolve ambiguity.

**If you see the word *unless* when you are doing a translation, replace it with the words *unless possibly if*.**

If we rephrase the sentence as:

```text
I will bike to work *unless possibly if* it is raining.
```

We see that there is clearly NO promise about whether I will bike in the rain. I might, or I might not -- but the only thing that I am promising is that I *will* bike if it is *not* raining, or that IF it's not raining, THEN I will bike. With that in mind, we can complete our translation:

```text
 ¬q →  ¬p
```

Equivalently, if we don't bike, then we are certain that it must be raining -- since we have promised to ride our bike every other time. We can also write:

```text
 ¬p →  ¬q
```