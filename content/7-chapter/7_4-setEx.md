---
title: "Set example"
pre: "7.4. "
weight: 83
date: 2018-08-24T10:53:26-05:00
---

Claim: If {{< math >}}$n${{< /math >}} is a positive integer greater than or equal to 2, then a set with {{< math >}}$n${{< /math >}} elements has {{< math >}}$\dfrac{n(n-1)}{2}${{< /math >}}possible subsets of size 2.

### Try it out

Suppose {{< math >}}$n = 3${{< /math >}}, and our set contains the elements {{< math >}}$(a, b, c)${{< /math >}}. There are 3 possible subsets of size 2: {{< math >}}$(a, b)${{< /math >}}, {{< math >}}$(a, c)${{< /math >}}, and {{< math >}}$(b, c)${{< /math >}}. We also have that {{< math >}}$\dfrac{3(3-1)}{2} = 3${{< /math >}}.

### Induction proof

We will use mathematical induction to prove our claim for all positive integers {{< math >}}$n${{< /math >}} greater than or equal to 2.

#### Base case

We must prove that the property holds for the smallest such integer, {{< math >}}$n = 2${{< /math >}}. We must show that a set with two elements contains {{< math >}}$\dfrac{2(2-1)}{2} = 1${{< /math >}} possible subsets of size 2. If a set has just two elements, then there is only one possible subset of size 2 -- the subset that contains both elements. Thus the base case holds.

#### Inductive step

We assume the inductive hypothesis - that a set with {{< math >}}$n${{< /math >}} elements has {{< math >}}$\dfrac{n(n-1)}{2}${{< /math >}} possible subsets of size 2. We must prove that a set with {{< math >}}$n + 1${{< /math >}} elements has {{< math >}}$\dfrac{(n+1)((n+1)-1)}{2} = \dfrac{n(n+1)}{2}${{< /math >}}possible subsets of size 2.

Introducing a new element to a set with $n$ elements yields {{< math >}}$n${{< /math >}}more 2-element subsets, as the new element could pair with each of the original elements.

A set with {{< math >}}$n+1${{< /math >}} elements contains all the original {{< math >}}$\dfrac{n(n-1)}{2}${{< /math >}} size-2 elements from the size-{{< math >}}$n${{< /math >}} set, plus the {{< math >}}$n${{< /math >}} new subsets described above. 

We have that:

```math
$$
\dfrac{n(n-1)}{2} + n = \dfrac{n(n-1)+2n}{2}
$$
$$
= \dfrac{n(n-1+2)}{2}
$$
$$
= \dfrac{n(n+1)}{2}
$$
```

Thus the inductive hypothesis holds.