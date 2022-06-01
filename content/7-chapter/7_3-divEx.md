---
title: "Divisibility example"
pre: "7.3. "
weight: 82
date: 2018-08-24T10:53:26-05:00
---

Claim: If $n$ is a positive integer, then $6^{n} - 1$ is divisible by 5.

### Try it out

Let's see if the property holds for several sample numbers. When $n = 3$, we have that $6^{3} - 1 = 216 - 1 = 215$. Since $215$ ends with a 5, it is clearly divisible by 5.

As another test, suppose $n = 5$. We have that $6^{5} - 1 = 7776 - 1 = 7775$, which is also divisible by 5.

### Induction proof

We will use mathematical induction to prove that if $n$ is a positive integer, then $6^{n} - 1$ is divisible by 5.

#### Base case

We must prove that the property holds for the smallest positive integer, $n = 1$, that is, that $6^{1} - 1$ is divisible by 5. We have that $6^{1} - 1 = 6 - 1 = 5$. 5 is divisible by 5, so the base case holds.

#### Inductive step

We assume the inductive hypothesis - that $6^{n} - 1$ is divisible by 5 for some positive integer $n$. We must prove that $6^{n+1} - 1$ is also divisible by 5. We have that:

$$
6^{n+1} - 1 = 6(6^{n}) - 1 \tag{1}
$$
$$
= 6(6^{n}) - 6 + 5 \tag{2}
$$
$$
= 6(6^{n} - 1) + 5 \tag{3}
$$

Since $6^{n} - 1$ is divisible by 5 from our inductive hypothesis, any multiple of it is also divisible by 5. Thus, $6(6^{n} - 1)$ is divisible by 5. Adding 5 to a number that is a multiple of 5 yields another multiple of 5. Thus $6(6^{n} - 1) + 5$ is divisible by 5, so our inductive step holds.