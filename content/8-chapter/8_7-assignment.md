---
title: "Assignment Statements"
pre: "8.7. "
weight: 96
date: 2018-08-24T10:53:26-05:00
---

Assignment statements in a program come in two forms -- with and without mutations. Assignments without mutation are those that give a value to a variable without using the old value of that variable. Assignments with mutation are variable assignments that use the old value of a variable to calculate a value for the variable.

For example, an increment statement like `x = x + 1` MUTATES the value of `x` by updating its value to be one bigger than it was before. In order to make sense of such a statement, we need to know the previous value of `x`.

In contrast, a statemnet like `y = x + 1` assigns to `y` one more than the value in `x`. We do not need to know the previous value of `y`, as we are not using it in the assignment statement. (We do need to know the value of `x`).

##
