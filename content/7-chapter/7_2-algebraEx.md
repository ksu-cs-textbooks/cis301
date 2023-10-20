---
title: "Algebra example"
pre: "7.2. "
weight: 81
date: 2018-08-24T10:53:26-05:00
---

Claim: The sum of the first {{< math >}}$n${{< /math >}} odd numbers is {{< math >}}$n^2${{< /math >}}. We will refer to this claim as {{< math >}}$P(n)${{< /math >}}.

### Try it out

Before proving {{< math >}}$P(n)${{< /math >}} with mathematical induction, let's see if the property holds for some sample values. The sum of the first 3 odd numbers is {{< math >}}$1 + 3 + 5 = 9${{< /math >}}. We also have that {{< math >}}$3^2 = 9${{< /math >}}.

The sum of the first 7 odd numbers is {{< math >}}$1 + 3 + 5 + 7 + 9 + 11 + 13 = 49${{< /math >}}. We also have that {{< math >}}$7^2 = 49${{< /math >}}.

Another way to express the sum of the first {{< math >}}$n${{< /math >}} odd numbers is: {{< math >}}$1 + 3 + ... + (2n - 1)${{< /math >}}. For example, when {{< math >}}$n${{< /math >}} is 4, we have that {{< math >}}$2n - 1 = 7${{< /math >}}. The sum of the first {{< math >}}$4${{< /math >}} odd numbers is {{< math >}}$1 + 3 + 5 + 7${{< /math >}}.

### Induction proof

We wish to use mathematical induction to prove that {{< math >}}$P(n)${{< /math >}} holds for all positive integers {{< math >}}$n${{< /math >}} That is, that the sum of the first {{< math >}}$n${{< /math >}} odd numbers is {{< math >}}$n^2${{< /math >}}:

```math
$$
\begin{aligned}
1 + 3 + ... + (2n - 1) = n^2
\end{aligned}
$$
```

<br>
<br>

We will refer to {{< math >}}$1 + 3 + ... + (2n - 1)${{< /math >}} as {{< math >}}$LHS_n${{< /math >}} and we will refer to {{< math >}}$n^2${{< /math >}} as {{< math >}}$RHS_n${{< /math >}}. To prove that {{< math >}}$P(n)${{< /math >}} holds for some positive integer {{< math >}}$n${{< /math >}}, we must prove that {{< math >}}$LHS_n = RHS_n${{< /math >}}.

#### Base case

We must prove that {{< math >}}$P(n)${{< /math >}} holds for the smallest positive integer, {{< math >}}$n = 1${{< /math >}}, that is, that {{< math >}}$LHS_1 = RHS_1${{< /math >}}  The sum the first 1 odd integer is just 1, so we have that {{< math >}}$LHS_1 = 1${{< /math >}}. We also have that {{< math >}}$RHS_1 = 1^2 = 1${{< /math >}}.

We have that {{< math >}}$LHS_1 = RHS_1${{< /math >}}. Thus {{< math >}}$P(1)${{< /math >}} is true, so the base case holds.

#### Inductive step

We assume the inductive hypothesis - that {{< math >}}$P(k)${{< /math >}} holds for some arbitrary positive integer {{< math >}}$k${{< /math >}}. In other words, we assume that {{< math >}}$LHS_k = RHS_k${{< /math >}} for our arbitrary {{< math >}}$k${{< /math >}}. We must prove that {{< math >}}$P(k+1)${{< /math >}} also holds -- i.e., that {{< math >}}$LHS_{k+1} = RHS_{k+1}${{< /math >}}. We have that:

```math
$$
LHS_{k+1} = 1 + 3 + ... + (2k - 1) + (2(k + 1) - 1) \tag{1}
$$
$$
= LHS_k + (2(k + 1) - 1) \tag{2}
$$
$$
= RHS_k + (2(nk + 1) - 1) \tag{3}
$$
$$
= k^2 + (2(k + 1) - 1) \tag{4}
$$
$$
= k^2 + 2k + 2 - 1 \tag{5}
$$
$$
= k^2 + 2k + 1 \tag{6}
$$
$$
= (k+1)^2 \tag{7}
$$
$$
= RHS_{k+1} \tag{8}
$$
```

Thus {{< math >}}$LHS_{k+1} = RHS_{k+1}${{< /math >}}, so we have proved {{< math >}}$P(k+1)${{< /math >}}. The inductive step holds.

We conclude that for all positive integers {{< math >}}n${{< /math >}}, {{< math >}}$P(n)${{< /math >}} holds  -- that is, that:

```math
$$
\begin{aligned}
1 + 3 + ... + (2n - 1) = n^2
\end{aligned}
$$
```