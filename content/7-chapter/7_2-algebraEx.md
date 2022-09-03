---
title: "Algebra example"
pre: "7.2. "
weight: 81
date: 2018-08-24T10:53:26-05:00
---

Claim: The sum of the first {{< math >}}$n${{< /math >}} odd numbers is {{< math >}}$n^2${{< /math >}}.

### Try it out

Before proving this claim with mathematical induction, let's see if the property holds for some sample values. The sum of the first 3 odd numbers is {{< math >}}$1 + 3 + 5 = 9${{< /math >}}. We also have that {{< math >}}$3^2 = 9${{< /math >}}.

The sum of the first 7 odd numbers is {{< math >}}$1 + 3 + 5 + 7 + 9 + 11 + 13 = 49${{< /math >}}. We also have that {{< math >}}$7^2 = 49${{< /math >}}.

Another way to express the sum of the first {{< math >}}$n${{< /math >}} odd numbers is: {{< math >}}$1 + 3 + ... + (2n - 1)${{< /math >}}. For example, when {{< math >}}$n${{< /math >}} is 4, we have that {{< math >}}$2n - 1 = 7${{< /math >}}. The sum of the first {{< math >}}$4${{< /math >}} odd numbers is {{< math >}}$1 + 3 + 5 + 7${{< /math >}}.

### Induction proof

We wish to use mathematical induction to prove that, for all positive integers {{< math >}}$n${{< /math >}}, the sum of the first {{< math >}}$n${{< /math >}} odd numbers is {{< math >}}$n^2${{< /math >}}. That is, that:

```math
$$
\begin{aligned}
1 + 3 + ... + (2n - 1) = n^2
\end{aligned}
$$
```

<br>
<br>

We will refer to {{< math >}}$1 + 3 + ... + (2n - 1)${{< /math >}} as {{< math >}}$LHS_n${{< /math >}} and we will refer to {{< math >}}$n^2${{< /math >}} as {{< math >}}$RHS_n${{< /math >}}. We must prove that {{< math >}}LHS_n = RHS_n${{< /math >}} for all positive integers *n*.

#### Base case

We must prove that the property holds for the smallest positive integer, {{< math >}}$n = 1${{< /math >}}, that is, that {{< math >}}$LHS_1 = RHS_1${{< /math >}}  The sum the first 1 odd integer is just 1, so we have that {{< math >}}$LHS_1 = 1${{< /math >}}. We also have that {{< math >}}$RHS_1 = 1^2 = 1${{< /math >}}. Thus {{< math >}}$LHS_1 = RHS_1${{< /math >}}, so the base case holds.

#### Inductive step

We assume the inductive hypothesis - that {{< math >}}$LHS_n = RHS_n${{< /math >}} for some positive integer {{< math >}}$n${{< /math >}}. We must prove that {{< math >}}$LHS_{n+1} = RHS_{n+1}${{< /math >}}. We have that:

```math
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
```

Thus {{< math >}}$LHS_{n+1} = RHS_{n+1}${{< /math >}}, so the inductive step holds.