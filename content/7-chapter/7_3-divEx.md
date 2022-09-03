---
title: "Divisibility example"
pre: "7.3. "
weight: 82
date: 2018-08-24T10:53:26-05:00
---

Claim: If {{< math >}}$n${{< /math >}} is a positive integer, then {{< math >}}$6^{n} - 1${{< /math >}} is divisible by 5.

### Try it out

Let's see if the property holds for several sample numbers. When {{< math >}}$n = 3${{< /math >}}  we have that {{< math >}}$6^{3} - 1 = 216 - 1 = 215${{< /math >}}. Since {{< math >}}$215${{< /math >}} ends with a 5, it is clearly divisible by 5.

As another test, suppose {{< math >}}$n = 5${{< /math >}}. We have that {{< math >}}$6^{5} - 1 = 7776 - 1 = 7775${{< /math >}}, which is also divisible by 5.

### Induction proof

We will use mathematical induction to prove that if {{< math >}}$n${{< /math >}} is a positive integer, then {{< math >}}$6^{n} - 1${{< /math >}} is divisible by 5.

#### Base case

We must prove that the property holds for the smallest positive integer, {{< math >}}$n = 1${{< /math >}}  that is, that {{< math >}}$6^{1} - 1${{< /math >}} is divisible by 5. We have that {{< math >}}$6^{1} - 1 = 6 - 1 = 5${{< /math >}} is divisible by 5, so the base case holds.

#### Inductive step

We assume the inductive hypothesis - that {{< math >}}$6^{n} - 1${{< /math >}} is divisible by 5 for some positive integer {{< math >}}$n${{< /math >}}. We must prove that {{< math >}}$6^{n+1} - 1${{< /math >}} is also divisible by 5. We have that:

```math
$$
6^{n+1} - 1 = 6(6^{n}) - 1 \tag{1}
$$
$$
= 6(6^{n}) - 6 + 5 \tag{2}
$$
$$
= 6(6^{n} - 1) + 5 \tag{3}
$$
```

Since {{< math >}}$6^{n} - 1${{< /math >}} is divisible by 5 from our inductive hypothesis, any multiple of it is also divisible by 5. Thus, {{< math >}}$6(6^{n} - 1)${{< /math >}} is divisible by 5. Adding 5 to a number that is a multiple of 5 yields another multiple of 5. Thus {{< math >}}$6(6^{n} - 1) + 5${{< /math >}} is divisible by 5, so our inductive step holds.