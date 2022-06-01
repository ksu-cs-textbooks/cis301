---
title: "Set example"
pre: "7.4. "
weight: 83
date: 2018-08-24T10:53:26-05:00
---

Claim: If $n$ is a positive integer greater than or equal to 2, then a set with $n$ elements has $\dfrac{n(n-1)}{2}$ possible subsets of size 2.

### Try it out

Suppose $n = 3$, and our set contains the elements $(a, b, c)$. There are 3 possible subsets of size 2: $(a, b)$, $(a, c)$, and $(b, c)$. We also have that $\dfrac{3(3-1)}{2} = 3$.

### Induction proof

We will use mathematical induction to prove our claim for all positive integers $n$ greater than or equal to 2.

#### Base case

We must prove that the property holds for the smallest such integer, $n = 2$. We must show that a set with two elements contains $\dfrac{2(2-1)}{2} = 1$ possible subsets of size 2. If a set has just two elements, then there is only one possible subset of size 2 -- the subset that contains both elements. Thus the base case holds.

#### Inductive step

We assume the inductive hypothesis - that a set with $n$ elements has $\dfrac{n(n-1)}{2}$ possible subsets of size 2. We must prove that a set with $n + 1$ elements has $\dfrac{(n+1)((n+1)-1)}{2} = \dfrac{n(n+1)}{2}$ possible subsets of size 2.

Introducing a new element to a set with $n$ elements yields $n$ more 2-element subsets, as the new element could pair with each of the original elements.

A set with $n+1$ elements contains all the original $\dfrac{n(n-1)}{2}$ size-2 elements from the size-$n$ set, plus the $n$ new subsets described above. 

We have that:

$$
\dfrac{n(n-1)}{2} + n = \dfrac{n(n-1)+2n}{2}
$$
$$
= \dfrac{n(n-1+2)}{2}
$$
$$
= \dfrac{n(n+1)}{2}
$$

Thus the inductive hypothesis holds.