---
title: "Induction Process"
pre: "7.1. "
weight: 80
date: 2018-08-24T10:53:26-05:00
header-includes:
    - \usepackage{amsmath}
---

*Mathematical induction* allows us to prove that every nonnegative integer satisfies a certain property. In this chapter, we will use mathematical inductive to prove several mathematical claims. When we reach the next several chapters on programming logic, we will see that mathematical induction is very similar to the process of proving correctness for programs with loops.

## Domino effect

To prove that a property is true for an arbitrary nonnegative integer $n$ using mathematical induction, we must show two things:

1. **Base case**. We must prove that the property is true for the smallest possible value of $n$. Usually this is $n = 0$ or $n = 1$, but occasionally we will define a property for all values greater than or equal to 2, or some bigger number. 

2. **Inductive step**. We assume the property holds for some arbitrary nonnegative integer $n$. This assumption is called the *inductive hypothesis*. Then, we must show that the property still holds for $n + 1$.

How do these two steps prove anything at all? Suppose we are proving that a property holds for all positive integers $n$. In the base case, we prove that the property holds when $n = 1$. Proving the inductive step allows us to say that whenever the property holds for some number, then it also holds for the number right after that. Since we already know the the property holds when $n = 1$, then the inductive step allows us to infer that the property still holds when $n = 2$. And at that point we know the property holds for $n = 2$, so the inductive step again allows us to infer that the property holds for $n = 3$, etc.

Think of mathematical inductive like a line of dominoes. The "base case" tells us that the first domino will fall, and the "inductive step" tells us that if one domino falls, then the one right after it will fall as well. From these two pieces, we can conclude that the entire line of dominoes will fall (i.e., that the property will hold for the entire set of numbers).

## Summation property

There is a well-known formula for adding all positive integers up to some bound, $n$:

$$
\begin{aligned}
1 + 2 + ... + n = \dfrac{n(n+1)}{2}
\end{aligned}
$$

<br>
<br>

To see how this works, suppose $n = 3.$ We have that $1 + 2 + 3 = 6$, and also that $\dfrac{3(3+1)}{2} = 6$. 

Suppose instead that $n = 7$. We have that $1 + 2 + 3 + 4 + 5 + 6 + 7 = 28$, and also that $\dfrac{7(7+1)}{2} = 28$. 

## First induction proof

We wish to use mathematical induction to prove that, for all positive integers $n$, the sum of all integers from 1 to $n$ is  $\dfrac{n(n+1)}{2}$. That is, that:

$$
\begin{aligned}
1 + 2 + ... + n = \dfrac{n(n+1)}{2}
\end{aligned}
$$

<br>
<br>

We will refer to $1 + 2 + ... + n$ as $LHS_n$ and we will refer to $\dfrac{n(n+1)}{2}$ as $RHS_n$. We must prove that $LHS_n = RHS_n$ for all positive integers $n$.

### Base case

We must prove that the property holds for the smallest positive integer, $n = 1$, that is, that $LHS_1 = RHS_1$  The sum of all integers from 1 to 1 is just 1, so we have that $LHS_1 = 1$. We have that:

$$
\begin{aligned}
RHS_1 = \dfrac{1(1+1)}{2} = 1
\end{aligned}
$$

<br>

Thus $LHS_1 = RHS_1$, so the base case holds.

### Inductive step

We assume the inductive hypothesis - that $LHS_n = RHS_n$ for some positive integer $n$. We must prove that $LHS_{n+1} = RHS_{n+1}$. We have that:

$$
LHS_{n+1} = 1 + 2 + ... + n + (n + 1) \tag{1}
$$
$$
= LHS_n + (n + 1) \tag{2} 
$$
$$
= RHS_n + (n + 1) \tag{3}
$$
$$
= \dfrac{n(n+1)}{2} + (n + 1) \tag{4}
$$
$$
= \dfrac{n(n+1)}{2} + \dfrac{2(n+1)}{2} \tag{5}
$$
$$
= \dfrac{(n+1)(n + 2)}{2} \tag{6}
$$
$$
= \dfrac{(n+1)((n + 1) + 1)}{2} \tag{7}
$$
$$
= RHS_{n+1} \tag{8}
$$

Thus $LHS_{n+1} = RHS_{n+1}$, so the inductive step holds.

## Inductive step explanation

In line 2 of the proof above we saw that $1 + 2 + ... + n$ was really $LHS_n$, so we made that substitution. Then in line 3, we used our inductive hypothesis - that $LHS_n = RHS_n$, and substituted $RHS_n$ for $LHS_n$. Since we had that $RHS_n = \dfrac{n(n+1)}{2}$, we made that substitution on line 4. 

From lines 5 to 7, we did algebraic manipulations to combine our terms and work towards the form of $RHS_{n+1}$. 