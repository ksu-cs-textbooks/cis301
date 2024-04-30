---
title: "Intro to Sequences"
pre: "10.2 "
weight: 111
date: 2018-08-24T10:53:26-05:00
---

*Sequences* in Logika are fairly similar to lists in Python and arrays in Java and C#. As with lists and arrays, Logika sequences are ordered. Each element is stored at a particular (zero-based) position, and there are no gaps in the sequence.

Logika can work with sequences of integers (type `ZS`).

## Sequence syntax

We can create new sequence variables like this:

```text
// #Sireum #Logika
//@Logika: --background save
import org.sireum._

//creates the sequence (5, 10, 15)
var seq: ZS = ZS(5,10,15)

//creates an empty sequence of integers
var empty: ZS = ZS()
```

Given the following sequence:

```text
var a: ZS = ZS(1, 2, 3)
```

Here is a table of the different sequence operations we will use in this course:

| Operation | Explanation | 
| --- | --- | 
| Indexing: `a(pos)` | Accesses the value in the sequence at position `pos`. <br> Sequences are zero-based, and Logika will show an error if you have not proven (or if it cannot infer, in symexe mode) that the position lies within the sequence range. <br><br> For example, `a(0)` is 1. <br> `a(0) = 11` would change the first value to be 11, so the sequence would be `(11,2,3)`. <br> `a(3)` would give an error, as position 3 is past the end of the sequence. |
|  Size: `a.size` | Evaluates to the number of elements in the sequence: `a.size == 3`| 
| Reassignment | Sequences instantiated as `var` can be reassigned. <br><br> For example, after `a = ZS(5,6)`, `a` is now `(5,6)`. |

## Sample program with a sequence

Here is a sample program that uses a sequence. The `makeFirstZero` function sets the first element in a sequence to 0:

```text
// #Sireum #Logika
//@Logika: --background save
import org.sireum._

//"Unit" is like a void return type
def makeFirstZero(seq: ZS): Unit = {
    seq(0) = 0
}

///// Test code ///////////

var nums: ZS = ZS(1,2,3)

makeFirstZero(nums)
assert(nums == ZS(0,2,3))
```

This program will not be verified as we have not yet provided a function contract for `makeZeroFirst`. We will complete the verification for the program later in the section.

## Predicate logic statements with sequences

When we write function contracts and loop invariants with sequences, we will need to make statements about *all* or *some* elements in a sequence. We can do this with predicate logic statements.

### Statements about all sequence elements

As we did in chapters 4 and 5, we will use the universal (`∀`) quantifier for statements involving all elements in a sequence. The basic forms of specifying some claim `P(a(x))` holds for every element in a sequence `a` are:

| Statement | Explanation | 
| --- | --- | 
| `∀ (lower to upper)(x => P(a(x)))` | `P(a(x))` holds for every element in `a` from position `lower` to position `upper` (including both `lower` and `upper`) |
| `∀ (lower until upper)(x => P(a(x)))` | `P(a(x))` holds for every element in `a` from position `lower` up to but not including position `upper` (`lower` but not `upper`) |

Here are several sample claims and explanations about integer sequence `a`:

| Claim | Explanation | 
| --- | --- | 
| `∀ (0 until a.size)(x => a(x) > 0)` | Every element in `a` is greater than 0 |
| `∀ (1 to 3)(x => a(x) == 0)` | All elements in `a` between positions 1 and 3 (inclusive of 1 and 3) have value 0 |
| `∀ (0 until a.size)(x => (a(x) < 0 → a(x) == -10)` | All negative elements in `a` have value -10 |

### Statements about some sequence elements

We will use the existential (`∃`) quantifier for statements involving one or more elements in a sequence. The basic forms of specifying claims is the same as for the universal quantifier, but using the existential quantifier instead of the universal quantifier.

Here are several sample claims and explanations about integer sequences `a` and `b`:

| Claim | Explanation | 
| --- | --- | 
| `∃ (0 until a.size)(x => a(x) > 0)` | There is an element in `a` that is greater than 0 |
| `∃ (2 to 4)(x => a(x) == a(x-1) * 2)` | There is an element in `a` between positions 2 and 4 (inclusive) that is twice as big as the previous element |
| `∀ (0 until a.size)(x => (∃ (0 until b.size) (y => a(x) == b(y)))` | Every value in `a` appears somewhere in `b` |

### Statements about initial and current sequence values

When we write preconditions, postconditions, and loop invariants involving sequences, we will want a way to distinguish between the INITIAL value a sequence had at the beginning of a function and the CURRENT value for that same sequence. We will use the notation `In(sequenceName)` to mean the value the sequence `sequenceName` had at the beginning of a function. In contrast, using just `sequenceName` refers to the value the sequence has right now (or, in the case of a postcondition, the value a sequence is promised to have at the end of a function).

For example, the postcondition:

```text
∀ (0 until a.size)(i => a(i) == In(a)(i) + 1
```

Means that when the function ends, every value in the sequence `a` (so each element `a(i)`) will be one bigger than the initial value at the same position (`In(a)(i) + 1`).