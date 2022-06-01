---
title: "Algebra example"
pre: "7.2. "
weight: 81
date: 2018-08-24T10:53:26-05:00
---

Claim: The sum of the first $n$ odd numbers is $n^2$.

### Try it out

Before proving this claim with mathematical induction, let's see if the property holds for some sample values. The sum of the first 3 odd numbers is $1 + 3 + 5 = 9$. We also have that $3^2 = 9$.

The sum of the first 7 odd numbers is $1 + 3 + 5 + 7 + 9 + 11 + 13 = 49$. We also have that $7^2 = 49$.

Another way to express the sum of the first $n$ odd numbers is: $1 + 3 + ... + (2n - 1)$. For example, when $n$ is 4, we have that $2n - 1 = 7$. The sum of the first $4$ odd numbers is $1 + 3 + 5 + 7$.

### Induction proof

We wish to use mathematical induction to prove that, for all positive integers $n$, the sum of the first $n$ odd numbers is $n^2$. That is, that:

$$
\begin{aligned}
1 + 3 + ... + (2n - 1) = n^2
\end{aligned}
$$

<br>
<br>

We will refer to $1 + 3 + ... + (2n - 1)$ as $LHS_n$ and we will refer to $n^2$ as $RHS_n$. We must prove that $LHS_n = RHS_n$ for all positive integers *n*.

#### Base case

We must prove that the property holds for the smallest positive integer, $n = 1$, that is, that $LHS_1 = RHS_1$  The sum the first 1 odd integer is just 1, so we have that $LHS_1 = 1$. We also have that $RHS_1 = 1^2 = 1$. Thus $LHS_1 = RHS_1$, so the base case holds.

#### Inductive step

We assume the inductive hypothesis - that $LHS_n = RHS_n$ for some positive integer $n$. We must prove that $LHS_{n+1} = RHS_{n+1}$. We have that:

$$
LHS_{n+1} = 1 + 3 + ... + (2n - 1) + (2(n + 1) - 1) \tag{1}
$$
$$
= LHS_n + (2(n + 1) - 1) \tag{2}
$$
$$
= RHS_n + (2(n + 1) - 1) \tag{3}
$$
$$
= n^2 + (2(n + 1) - 1) \tag{4}
$$
$$
= n^2 + 2n + 2 - 1 \tag{5}
$$
$$
= n^2 + 2n + 1 \tag{6}
$$
$$
= (n+1)^2 \tag{7}
$$
$$
= RHS_{n+1} \tag{8}
$$

Thus $LHS_{n+1} = RHS_{n+1}$, so the inductive step holds.