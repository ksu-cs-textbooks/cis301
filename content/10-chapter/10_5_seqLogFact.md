---
title: "Logika Facts, revisited"
pre: "10.5 "
weight: 114
date: 2018-08-24T10:53:26-05:00
---

As in other programs with loops, programs with sequences sometimes necessitate the use of a *Logika fact* to describe how a particular value is calculated. For example, consider the following program that finds and returns the sum of all elements in an integer sequence:

```text
import org.sireum.logika._

def seqSum(list: ZS): Z = {
    var i: Z = 0
    var total: Z = 0

    while (i < list.size) {
        total = total + list(i)
        i = i + 1
    }

    return total
}

////////////// Calling code ///////////////////

var test: ZS = ZS(1,2,3,4)
var added: Z = seqSum(test)

assert(added == 10)
```

In the `seqSum` function contract, we need to describe that the return value equals the sum of all sequence elements -- that is, that `result == list(0) + list(1) + ... + list(list.size - 1)`. This is the same situation that we encountered when trying to specify something like a factorial. We know the pattern that we want to describe, but aren't able to do so without using the "..." notation. When you need to describe a pattern in this way, you will almost always want to use a Logika fact.

## Blueprint for Logika facts with sequences

When writing a Logika fact that works with a sequence, we will have this general recursive definition:

- Base case: we have already processed all elements in the sequence
- Recursive case: process one sequence element, and recursively process the rest

We need some way to track what element we are ready to process, so in addition to having the Logika proof function take a sequence parameter, we will also have it take a parameter that stores how many elements we have left to process. We will use this template for our Logika fact:

```text
l"""{
    fact
        def factName(seqName: seqType, count: Z): returnType
            = (baseCaseValue), if count == 0                    (factName0)
            = (recursiveCaseValue), if count > 0                (factNameN)
}"""
```

Where we have that:

- `factName` is the name we give our Logika proof function
- `seqName` is the name of our sequence parameter, and `seqType` is its type (either `ZS` or `ZB`)
- `returnType` is the type of what we are calculating (most likely `Z` or `B`)
- `count` is the number of items left to process in the sequence. We will see that we will initially pass our sequence size as this parameter, so that all elements in the sequence will be processed.
- `(baseCaseValue)` is the value we want for our base case -- if we have already processed all sequence elements
- `(recursiveCaseValue)` is the value we want for our recursive case. In this step, we want to process the current sequence element, `seqName(count-1)`, and then use the proof function to recursively evaluate the rest of the sequence (passing `count-1` as the number of items left to process).
- `(factName0)` is the name we are giving out base case definition, and `(factNameN)` is the name we are giving our recursive case definition. We will not need to refer to these name in our verification when we use symexe mode.

You may notice that this format is slightly different than the format we used for Logika facts in section 9.4. We could also write our sequence Logika facts in that format, using quantifiers to describe ranges, but you will find that the above format is much more straightforward.

## Logika fact for sequence sum

We are now ready to write a Logika fact to describe the calculation of adding all elements in a sequence -- the sum `list(0) + list(1) + ... + list(list.size - 1)` for some sequence `list`. We use the template above to write:

```text
l"""{
    fact
        def sumFact(seq: ZS, pos: Z): Z
            = 0, if pos == 0                                    (sum0)
            = seq(pos-1) + sumFact(seq, pos-1), if pos > 0          (sumN)
}"""
```

## How does the sequence sum fact work?

To see how this Logika proof function works for a particular sequence, seq is `ZS(5,4,2)`. If we were to calculate:

```text
sumFact(seq, seq.size)
```

Then `sumFact` would initially go into the recursive case, since `pos` is 3 (and is greater than 0). Thus we would have that:

```text
sumFact(seq, seq.size) = 2 + sumFact(seq, 2)
```

`sumFact(seq, 2)` would also go into the recursive case. Since `pos` is 2 in this case, it would evaluate to be: `4 + sumFact(seq, 1)`. Next, `sumFact(seq, 1)` would evaluate to be: `5 + sumFact(seq, 0)`, and then `sumFact(seq, 0)` would reach our base case and would evaluate to 0. 

Since `sumFact(seq, 0)` is 0, we now have that:

```text
sumFact(seq, 1) = 5 + sumFact(seq, 0) = 5 + 0 = 5
```

And then we can plug in 5 for `sumFact(seq, 1)` to get:

```text
sumFact(seq, 2) = 4 + sumFact(seq, 1) = 4 + 5 = 9
```

Lastly, we can use `sumFact(seq, 2) == 9` in our top-level calculation to get that:

```text
sumFact(seq, seq.size) = 2 + sumFact(seq, 2) = 2 + 9 = 11
```

And we see that `sumFact` has correctly described that the sum of all elements in our `ZS(5,4,2)` sequence is 11.

## Finishing the `seqSum`` example

Now that we have a Logika fact to describe the sum of all the elements in a sequence, we have enough information to write the postcondition and loop invariant for our `seqSum` function. 

For the function contract, we must consider:

- *Precondition*: this function will work for all sizes of sequences. For an empty sequence, it will correctly return a sum of 0. We can omit the `requires` clause from the function contract. 
- `modifies` clause: this function is NOT modifying its sequence parameter, so we can omit this clause
- *Postcondition*: the function is not changing the sequence, so we do not need to describe the final values of each sequence element. We *do* need to describe what value we are returning, and how it relates to the sequence. We want to use our `sumFact` proof function to describe that `result` (our return value) is the sum of all elements in `list` -- that is, that `result == sumFact(list, list.size)`.

For the loop invariant block, we notice that the loop is NOT changing the sequence. We must include:

- *What we are doing with each sequence element, and how that relates to another variable*. We can see that `total` tracks the sum of all elements in the sequence so far -- up to but not including position `i`. Similar to the postcondition, we want to use our `sumFact` proof function to claim that `small` is the sum of the first `i` elements in `list` -- that is, that `small == sumFact(list, i)`.
 - *Upper and lower bounds for position variables*. Here, `i` is our position variable. We see that it is initialized to 0, so we will claim that it is always greater than or equal to 0 and less than or equal to the list size.

 We can now complete the function contract and loop invariant for `seqSum`:

 ```text
import org.sireum.logika._

//What is this Logika fact saying?

//add all elements from position 0 up to but not including pos
//sum(seq, seq.size) - defines adding ALL elements in seq

l"""{
    fact
        def sum(seq: ZS, pos: Z): Z
            = 0, if pos == 0                                    (sum0)
            = seq(pos-1) + sum(seq, pos-1), if pos > 0          (sumN)
}"""

def seqSum(list: ZS): Z = {
    l"""{
        ensures
            result == sum(list, list.size)
    }"""

    var i: Z = 0
    var total: Z = 0

    while (i < list.size) {
        l"""{
            invariant
                //total is the sum of the first i elements
                //total = list(0) + list(1) + ... + list(i-1)
                total == sum(list, i)
                i >= 0
                i <= list.size
            modifies total, i
        }"""

        total = total + list(i)
        i = i + 1
    }

    return total
}

////////////// Calling code ///////////////////

var test: ZS = ZS(1,2,3,4)
var added: Z = seqSum(test)

assert(added == 10)
 ```

 If we test this program in Logika's symexe mode, the final assert will hold -- we have enough information to know exactly the sum of a specific sequence.