---
title: "Set example"
pre: "7.4. "
weight: 83
date: 2018-08-24T10:53:26-05:00
---

Claim: If {{< math >}}$n${{< /math >}} is a positive integer greater than or equal to 2, then a set with {{< math >}}$n${{< /math >}} elements has {{< math >}}$\dfrac{n(n-1)}{2}${{< /math >}} possible subsets of size 2. We will refer to this claim as {{< math >}}$P(n)${{< /math >}}.

### Try it out

Suppose {{< math >}}$n = 3${{< /math >}}, and our set contains the elements {{< math >}}$(a, b, c)${{< /math >}}. There are 3 possible subsets of size 2: {{< math >}}$(a, b)${{< /math >}}, {{< math >}}$(a, c)${{< /math >}}, and {{< math >}}$(b, c)${{< /math >}}. We also have that {{< math >}}$\dfrac{3(3-1)}{2} = 3${{< /math >}}.

### Induction proof

We wish to use mathematical induction to prove that {{< math >}}$P(n)${{< /math >}} holds for all integers {{< math >}}$n \geq 2${{< /math >}}. That is, that a set with {{< math >}}$n${{< /math >}} elements has {{< math >}}$\dfrac{n(n-1)}{2}${{< /math >}}possible subsets of size 2.

#### Base case

We must prove that {{< math >}}$P(n)${{< /math >}} holds for the smallest such integer, {{< math >}}$n = 2${{< /math >}}. We must show that a set with two elements contains {{< math >}}$\dfrac{2(2-1)}{2} = 1${{< /math >}} possible subsets of size 2. If a set has just two elements, then there is only one possible subset of size 2 -- the subset that contains both elements. This proves {{< math >}}$P(2)${{< /math >}}, so the base case holds.

#### Inductive step

We assume the inductive hypothesis - that {{< math >}}$P(k)${{< /math >}} holds for some arbitrary integer {{< math >}}$k \geq 2${{< /math >}}. In other words, we assume that a set with {{< math >}}$k${{< /math >}} elements has {{< math >}}$\dfrac{k(k-1)}{2}${{< /math >}} possible subsets of size 2. We must prove that {{< math >}}$P(k+1)${{< /math >}} also holds -- i.e., that a set with {{< math >}}$k + 1${{< /math >}} elements has:

```math
$$
\dfrac{(k+1)((k+1)-1)}{2} = \dfrac{k(k+1)}{2}
$$
```

possible subsets of size 2.

Introducing a new element to a set with {{< math >}}$k${{< /math >}} elements yields {{< math >}}$k${{< /math >}} additional 2-element subsets, as the new element could pair with each of the original elements.

A set with {{< math >}}$k+1${{< /math >}} elements contains all the original {{< math >}}$\dfrac{k(k-1)}{2}${{< /math >}} size-2 elements from the size-{{< math >}}$k${{< /math >}} set, plus the {{< math >}}$k${{< /math >}} new subsets described above. 

We have that:

```math
$$
\dfrac{k(k-1)}{2} + k = \dfrac{k(k-1)+2k}{2}
$$
$$
= \dfrac{k(k-1+2)}{2}
$$
$$
= \dfrac{k(k+1)}{2}
$$
```

We have proved {{< math >}}$P(k+1)${{< /math >}}. Thus the inductive hypothesis holds. 

We conclude that for all positive integers {{< math >}}$n \geq 2${{< /math >}}, {{< math >}}$P(n)${{< /math >}} holds -- that is, that a set with {{< math >}}$n${{< /math >}} elements has {{< math >}}$\dfrac{n(n-1)}{2}${{< /math >}} possible subsets of size 2. 
