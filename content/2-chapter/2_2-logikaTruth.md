---
title: "Truth Tables in Logika"
weight: 31
pre: "2.2. "
---

Now that we've seen the four basic logic gates and truth tables, we can put them together to build bigger truth tables for longer logical formulae.

## Operator precedence

Logical operators have a defined precedence (order of operations) just as arithmetic operators do. In arithmetic, parentheses have the highest precedence, followed by exponents, then multiplication and division, and finally addition and subtraction.

Here is the precedence of the logical operators, from most important (do first) to least important (do last):

1) Parentheses
2) Not operator, `¬`
3) And operator, `∧`
4) Or operator, `∨`
5) Implies operator, `→`

For example, in the statement `(p ∨ q) ∧ ¬p`, we would evaluate the operators in the following order:

1) The parentheses (which would resolve the `(p ∨ q)` expression)
2) The not, `¬`
3) The and, `∧`

Sometimes we have more than one of the same operator in a single statement. For example: `p ∨ q ∨ r`. Different operators have different rules for resolving multiple occurrences:

1) Multiple parentheses - the innermost parentheses are resolved first, working from inside out.
2) Multiple not ( `¬` ) operators -- the rightmost `¬` is resolved first, working from right to left. For example: `¬¬p` is equivalent to `¬(¬p)`.
3) Multiple and ( `∧` ) operators -- the leftmost `∧` is resolved first, working from left to right. For example, `p ∧ q ∧ r` is equivalent to `(p ∧ q) ∧ r`.
4) Multiple or ( `∨` ) operators -- the leftmost `∨` is resolved first, working from left to right. For example, `p ∨ q ∨ r` is equivalent to `(p ∨ q) ∨ r`.
5) Multiple implies ( `→` ) operators -- the rightmost `→` is resolved first, working from right to left. For example, `p → q → r` is equivalent to `p → (q → r)`.

## Top-level operator
In a logical statement, the *top-level operator* is the operator that is applied last (after following the precedence rules above). 

For example, in the statement:

`p ∨ q → ¬p ∧ r`

We would evaluate first the `¬`, then the `∧`, then the `∨`, and lastly the `→`. Thus the `→` is the top-level operator.

## Classifying truth tables

In our study of logic, it will be convenient to characterize logical formula with a description of their truth tables. We will classify each logical formula in one of three ways:

- *Tautology* - when all truth assignments for a logical formula are true
- *Contradictory* - when all truth assignments for a logical formula are false
- *Contingent* - when some truth assignments for a logical formula are true and some are false.

For example, `p ∨ ¬ p` is a *tautology*. Whether `p` is true or false, `p ∨ ¬ p` is always true.

On the other hand, `p ∧ ¬ p` is *contradictory*. Whether `p` is true or false, `p ∧ ¬ p` is always false.

Finally, something like `p ∨ q` is *contingent*. When `p` and `q` are both false, then `p ∨ q` is false. However, `p ∨ q` is true in every other case.

 If all truth assignments for a logical formula are True, the formula is said to be a tautology.

## Logika syntax

## Examples