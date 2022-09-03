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

To prove that a property is true for an arbitrary nonnegative integer {{< math >}}$n${{< /math >}} using mathematical induction, we must show two things:

1. **Base case**. We must prove that the property is true for the smallest possible value of {{< math >}}$n${{< /math >}}. Usually this is {{< math >}}$n = 0${{< /math >}} or {{< math >}}$n = 1${{< /math >}}, but occasionally we will define a property for all values greater than or equal to 2, or some bigger number. 

2. **Inductive step**. We assume the property holds for some arbitrary nonnegative integer {{< math >}}$n${{< /math >}}. This assumption is called the *inductive hypothesis*. Then, we must show that the property still holds for {{< math >}}$n + 1${{< /math >}}.

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

We wish to use mathematical induction to prove that, for all positive integers {{< math >}}$n${{< /math >}}, the sum of all integers from 1 to {{< math >}}$n${{< /math >}} is {{< math >}}$\dfrac{n(n+1)}{2}${{< /math >}}. That is, that:

```math
$$
\begin{aligned}
1 + 2 + ... + n = \dfrac{n(n+1)}{2}
\end{aligned}
$$
```

<br>
<br>

We will refer to {{< math >}}$1 + 2 + ... + n${{< /math >}} as {{< math >}}$LHS_n${{< /math >}} and we will refer to {{< math >}}$\dfrac{n(n+1)}{2}${{< /math >}} as {{< math >}}$RHS_n${{< /math >}}. We must prove that {{< math >}}$LHS_n = RHS_n${{< /math >}} for all positive integers {{< math >}}$n${{< /math >}}.

### Base case

We must prove that the property holds for the smallest positive integer, {{< math >}}$n = 1${{< /math >}}, that is, that {{< math >}}$LHS_1 = RHS_1${{< /math >}}. The sum of all integers from 1 to 1 is just 1, so we have that {{< math >}}$LHS_1 = 1${{< /math >}}. We have that:

```math
$$
\begin{aligned}
RHS_1 = \dfrac{1(1+1)}{2} = 1
\end{aligned}
$$
```

<br>

Thus {{< math >}}$LHS_1 = RHS_1${{< /math >}}, so the base case holds.

### Inductive step

We assume the inductive hypothesis - that {{< math >}}$LHS_n = RHS_n${{< /math >}} for some positive integer {{< math >}}$n${{< /math >}}. We must prove that {{< math >}}$LHS_{n+1} = RHS_{n+1}${{< /math >}}. We have that:

```math
$$
LHS_{n+1} = 1 + 2 + ... + n + (n + 1) \tag{1}
$$
$$
= LHS_n + (n + 1) \tag{2} 
$$
$$
= RHS_n + (n + 1) \tag{3}
$$
$$
= \dfrac{n(n+1)}{2} + (n + 1) \tag{4}
$$
$$
= \dfrac{n(n+1)}{2} + \dfrac{2(n+1)}{2} \tag{5}
$$
$$
= \dfrac{(n+1)(n + 2)}{2} \tag{6}
$$
$$
= \dfrac{(n+1)((n + 1) + 1)}{2} \tag{7}
$$
$$
= RHS_{n+1} \tag{8}
$$
```

Thus {{< math >}}$LHS_{n+1} = RHS_{n+1}${{< /math >}}, so the inductive step holds.

## Inductive step explanation

In line 2 of the proof above we saw that {{< math >}}$1 + 2 + ... + n${{< /math >}} was really {{< math >}}$LHS_n${{< /math >}}, so we made that substitution. Then in line 3, we used our inductive hypothesis - that {{< math >}}$LHS_n = RHS_n${{< /math >}}, and substituted {{< math >}}$RHS_n${{< /math >}}for {{< math >}}$LHS_n${{< /math >}}. Since we had that {{< math >}}$RHS_n = \dfrac{n(n+1)}{2}${{< /math >}}, we made that substitution on line 4. 

From lines 5 to 7, we did algebraic manipulations to combine our terms and work towards the form of {{< math >}}$RHS_{n+1}${{< /math >}}. 