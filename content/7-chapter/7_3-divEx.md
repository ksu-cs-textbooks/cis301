---
title: "Divisibility example"
pre: "7.3. "
weight: 82
date: 2018-08-24T10:53:26-05:00
---

Claim: If {{< math >}}$n${{< /math >}} is a positive integer, then {{< math >}}$6^{n} - 1${{< /math >}} is divisible by 5. We will refer to this claim as {{< math >}}$P(n)${{< /math >}}.

### Try it out

Before proving {{< math >}}$P(n)${{< /math >}} with mathematical induction, let's see if the property holds for some sample values. When {{< math >}}$n = 3${{< /math >}}  we have that {{< math >}}$6^{3} - 1 = 216 - 1 = 215${{< /math >}}. Since {{< math >}}$215${{< /math >}} ends with a 5, it is clearly divisible by 5.

As another test, suppose {{< math >}}$n = 5${{< /math >}}. We have that {{< math >}}$6^{5} - 1 = 7776 - 1 = 7775${{< /math >}}, which is also divisible by 5.

### Induction proof

We wish to use mathematical induction to prove that {{< math >}}$P(n)${{< /math >}} holds for all positive integers {{< math >}}$n${{< /math >}}. That is, that {{< math >}}$6^{n} - 1${{< /math >}} is divisible by 5.

#### Base case

We must prove that {{< math >}}$P(n)${{< /math >}} holds for the smallest positive integer, {{< math >}}$n = 1${{< /math >}}, that is, that {{< math >}}$6^{1} - 1${{< /math >}} is divisible by 5. We have that {{< math >}}$6^{1} - 1 = 6 - 1 = 5${{< /math >}} is divisible by 5, so {{< math >}}$P(1)${{< /math >}} is true. The base case holds.

#### Inductive step

We assume the inductive hypothesis - that {{< math >}}$P(k)${{< /math >}} holds for some arbitrary positive integer {{< math >}}$k${{< /math >}}. In other words, we assume that {{< math >}}$6^{k} - 1${{< /math >}} is divisible by 5 for our arbitrary {{< math >}}$k${{< /math >}}. We must prove that {{< math >}}$P(k+1)${{< /math >}} also holds -- i.e., that {{< math >}}$6^{k+1} - 1${{< /math >}} is also divisible by 5. We have that:

```math
$$
6^{k+1} - 1 = 6(6^{k}) - 1 \tag{1}
$$
$$
= 6(6^{k}) - 6 + 5 \tag{2}
$$
$$
= 6(6^{k} - 1) + 5 \tag{3}
$$
```

Since {{< math >}}$6^{k} - 1${{< /math >}} is divisible by 5 from our inductive hypothesis, any multiple of it is also divisible by 5. Thus, {{< math >}}$6(6^{k} - 1)${{< /math >}} is divisible by 5. Adding 5 to a number that is a multiple of 5 yields another multiple of 5. Thus {{< math >}}$6(6^{k} - 1) + 5${{< /math >}} is divisible by 5, we have proved {{< math >}}$P(nk+1)${{< /math >}}. The inductive step holds.

We conclude that for all positive integers {{< math >}}n${{< /math >}}, {{< math >}}$P(n)${{< /math >}} holds -- that is, that {{< math >}}$6^{n} - 1${{< /math >}} is divisible by 5.

