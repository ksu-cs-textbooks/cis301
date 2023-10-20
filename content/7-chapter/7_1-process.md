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

To prove that a property {{< math >}}$P(n)${{< /math >}} is true for an arbitrary nonnegative integer {{< math >}}$n${{< /math >}} using mathematical induction, we must show two things:

1. **Base case**. We must prove that {{< math >}}$P(n)${{< /math >}} is true for the smallest possible value of {{< math >}}$n${{< /math >}}. Usually this is {{< math >}}$n = 0${{< /math >}} or {{< math >}}$n = 1${{< /math >}}, but occasionally we will define a property for all values greater than or equal to 2, or some bigger number. 

2. **Inductive step**. We assume the *inductive hypothesis* -- that {{< math >}}$P(k)${{< /math >}} holds for some arbitrary nonnegative integer {{< math >}}$k${{< /math >}}. Then, we must show that the property still holds for {{< math >}}$k + 1${{< /math >}}. In other words, we must prove that {{< math >}}$P(k) \rightarrow P(k + 1)${{< /math >}} -- that IF {{< math >}}$P(k)${{< /math >}} holds for some arbitrary nonnegative integer {{< math >}}$k${{< /math >}}, THEN {{< math >}}$P(k + 1)${{< /math >}} holds as well.

How do these two steps prove anything at all? Suppose we are proving that a property holds for all positive integers {{< math >}}$n${{< /math >}}. In the base case, we prove that the property holds when {{< math >}}$n = 1${{< /math >}}. Proving the inductive step allows us to say that whenever the property holds for some number, then it also holds for the number right after that. Since we already know the the property holds when {{< math >}}$n = 1${{< /math >}}, then the inductive step allows us to infer that the property still holds when {{< math >}}$n = 2${{< /math >}}. And at that point we know the property holds for {{< math >}}$n = 2${{< /math >}}, so the inductive step again allows us to infer that the property holds for {{< math >}}$n = 3${{< /math >}}, etc.

Think of mathematical inductive like a line of dominoes. The "base case" tells us that the first domino will fall, and the "inductive step" tells us that if one domino falls, then the one right after it will fall as well. From these two pieces, we can conclude that the entire line of dominoes will fall (i.e., that the property will hold for the entire set of numbers).

## Summation property

There is a well-known formula for adding all positive integers up to some bound, {{< math >}}$n${{< /math >}}:

```math
$$
\begin{aligned}
1 + 2 + ... + n = \dfrac{n(n+1)}{2}
\end{aligned}
$$
```

<br>
<br>

To see how this works, suppose {{< math >}}$n = 3${{< /math >}}. We have that {{< math >}}$1 + 2 + 3 = 6${{< /math >}}, and also that {{< math >}}$\dfrac{3(3+1)}{2} = 6${{< /math >}}. 

Suppose instead that {{< math >}}$n = 7${{< /math >}}. We have that {{< math >}}$1 + 2 + 3 + 4 + 5 + 6 + 7 = 28${{< /math >}}, and also that {{< math >}}$\dfrac{7(7+1)}{2} = 28${{< /math >}}. 

## First induction proof

Let {{< math >}}$P(n)${{< /math >}} be the equation: 

```math
$$
\begin{aligned}
1 + 2 + ... + n = \dfrac{n(n+1)}{2}
\end{aligned}
$$
```

We wish to use mathematical induction to prove that {{< math >}}$P(n)${{< /math >}} holds for all positive integers {{< math >}}$n${{< /math >}}.

<br>
<br>

We will refer to {{< math >}}$1 + 2 + ... + n${{< /math >}} as {{< math >}}$LHS_n${{< /math >}} and we will refer to {{< math >}}$\dfrac{n(n+1)}{2}${{< /math >}} as {{< math >}}$RHS_n${{< /math >}}. To prove that {{< math >}}$P(n)${{< /math >}} holds for some positive integer {{< math >}}$n${{< /math >}}, we must prove that {{< math >}}$LHS_n = RHS_n${{< /math >}}.

### Base case

We must prove that {{< math >}}$P(n)${{< /math >}} holds for the smallest positive integer, {{< math >}}$n = 1${{< /math >}}, that is, that {{< math >}}$LHS_1 = RHS_1${{< /math >}}. The sum of all integers from 1 to 1 is just 1, so we have that {{< math >}}$LHS_1 = 1${{< /math >}}. We also have that:

```math
$$
\begin{aligned}
RHS_1 = \dfrac{1(1+1)}{2} = 1
\end{aligned}
$$
```

<br>

We have that {{< math >}}$LHS_1 = RHS_1${{< /math >}}. Thus {{< math >}}$P(1)${{< /math >}} is true, so the base case holds.

### Inductive step

We assume the inductive hypothesis - that {{< math >}}$P(k)${{< /math >}} holds for some arbitrary positive integer {{< math >}}$k${{< /math >}}. In other words, we assume that {{< math >}}$LHS_k = RHS_k${{< /math >}} for our arbitrary {{< math >}}$k${{< /math >}}. We must prove that {{< math >}}$P(k+1)${{< /math >}} also holds -- i.e., that {{< math >}}$LHS_{k+1} = RHS_{k+1}${{< /math >}}. We have that:

```math
$$
LHS_{k+1} = 1 + 2 + ... + k + (k + 1) \tag{1}
$$
$$
= LHS_k + (k + 1) \tag{2} 
$$
$$
= RHS_k + (k + 1) \tag{3}
$$
$$
= \dfrac{k(k+1)}{2} + (k + 1) \tag{4}
$$
$$
= \dfrac{k(k+1)}{2} + \dfrac{2(k+1)}{2} \tag{5}
$$
$$
= \dfrac{(k+1)(k + 2)}{2} \tag{6}
$$
$$
= \dfrac{(k+1)((k + 1) + 1)}{2} \tag{7}
$$
$$
= RHS_{k+1} \tag{8}
$$
```

Thus {{< math >}}$LHS_{k+1} = RHS_{k+1}${{< /math >}}, so we have proved {{< math >}}$P(k+1)${{< /math >}}. The inductive step holds.

<br><br>

We conclude that for all positive integers {{< math >}}$n${{< /math >}}, {{< math >}}$P(n)${{< /math >}} holds  -- that is, that:

```math
$$
\begin{aligned}
1 + 2 + ... + n = \dfrac{n(n+1)}{2}
\end{aligned}
$$
```

## Inductive step explanation

In line 2 of the proof above we saw that {{< math >}}$1 + 2 + ... + k${{< /math >}} was really {{< math >}}$LHS_k${{< /math >}}, so we made that substitution. Then in line 3, we used our inductive hypothesis - that {{< math >}}$LHS_k = RHS_k${{< /math >}}, and substituted {{< math >}}$RHS_k${{< /math >}} for {{< math >}}$LHS_k${{< /math >}}. Since we had that {{< math >}}$RHS_k = \dfrac{k(k+1)}{2}${{< /math >}}, we made that substitution on line 4. 

From lines 5 to 7, we did algebraic manipulations to combine our terms and work towards the form of {{< math >}}$RHS_{k+1}${{< /math >}}. 