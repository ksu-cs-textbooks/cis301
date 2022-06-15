---
title: "Intro to Sequences"
pre: "10.2 "
weight: 111
date: 2018-08-24T10:53:26-05:00
---

*Sequences* in Logika are fairly similar to lists in Python and arrays in Java and C#. As with lists and arrays, Logika sequences are ordered. Each element is stored at a particular (zero-based) position, and there are no gaps in the sequence.

Logika sequences can either store integers (type `ZS`) or booleans (type `BS`).

## Sequence syntax

We can create new sequence variables like this:

```text
//creates the sequence (5, 10, 15)
var seq: ZS = ZS(5,10,15)

//creates the sequence (false, true)
var bools: BS = BS(false, true)

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
| Indexing: `a(pos)` | Accesses the value in the sequence at position `pos`. <br> Sequences are zero-based, and Logika will show an error if you have not proven (or if it cannot infer, in symexe mode) that the position lies within the sequence range. <br><br> For example, `a(0)` is 1. <br> `a(0) = 11` would change the first value to be 11, so the sequence would be `[11,2,3]`. <br> `a(3)` would give an error, as position 3 is past the end of the sequence. |
|  Size: `a.size` | Evaluates to the number of elements in the sequence: `a.size == 3`| 
| Reassignment | Sequences instantiated as `var` can be reassigned. <br><br> For example, after `a = ZS(5,6)`, `a` is now `[5,6]`. |

## Sample program with a sequence

Here is a sample Logika program that uses a sequence. The `makeFirstZero` function sets the first element in a sequence to 0::

```text
import org.sireum.logika._

//"Unit" is like a void return type
def makeFirstZero(seq: ZS): Unit = {
    seq(0) = 0
}

///// Test code ///////////

var nums: ZS = ZS(1,2,3)

makeFirstZero(nums)
assert(nums == ZS(0,2,3))
```

This program will not run (or be verified) as we have not yet provided a function contract for `makeZeroFirst`. We will complete the verification for the program later in the section.

## Predicate logic statements with sequences

When we write function contracts and loop invariants with sequences, we will need to make statements about *all* or *some* elements in a sequence. We can do this with predicate logic statements.

### Statements about all sequence elements

As we did in chapters 4 and 5, we will use the universal (`∀`) quantifier for statements involving all elements in a sequence. The basic forms of specifying some claim `P(a(x))` holds for every element in a sequence `a` are:

| Statement | Explanation | 
| --- | --- | 
| `∀ x: (lower..upper)  P(a(x))` | `P(a(x))` holds for every element in `a` from position `lower` to position `upper` (including both `lower` and `upper`) |
| `∀ x: (lower..<upper)  P(a(x))` | `P(a(x))` holds for every element in `a` from position `lower` up to but not including position `upper` (`lower` but not `upper`) |

Here are several sample claims and explanations about integer sequence `a`:

| Claim | Explanation | 
| --- | --- | 
| `∀ x: (0..<a.size) a(x) > 0` | Every element in `a` is greater than 0 |
| `∀ x: (1..3) a(x) == 0` | All elements in `a` between positions 1 and 3 (inclusive of 1 and 3) have value 0 |
| `∀ x: (0..<a.size) a(x) < 0 → a(x) == -10` | All negative elements in `a` have value -10 |

### Statements about some sequence elements

We will use the existential (`∃`) quantifier for statements involving one or more elements in a sequence. The basic forms of specifying claims is the same as for the universal quantifier, but using the existential quantifier instead of the universal quantifier.

Here are several sample claims and explanations about integer sequences `a` and `b`:

| Claim | Explanation | 
| --- | --- | 
| `∃ x: (0..<a.size) a(x) > 0` | There is an element in `a` that is greater than 0 |
| `∃ x: (2..4) a(x) == a(x-1) * 2` | There is an element in `a` between positions 2 and 4 (inclusive) that is twice as big as the previous element |
| `∀ x: (0..<a.size) (∃ y: (0..<b.size) a(x) == b(y))` | Every value in `a` appears somewhere in `b` |