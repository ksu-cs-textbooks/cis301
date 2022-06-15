---
title: "Sequences in Functions"
pre: "10.3 "
weight: 112
date: 2018-08-24T10:53:26-05:00
---

Sequences in Logika are passed to functions by reference. This means that if a function makes changes to a sequence parameter, then it will also change whatever sequence object was passed to the function. For example, the test code above passes `nums`, which has the value `ZS(1,2,3)`, to the `makeFirstZero` function. The `makeFirstZero` function changes the first position in its parameter (`seq`) to be 0, which means that the `nums` sequence in the test code will also have its first position set to 0 (making it have the value `ZS(0,2,3)`).

## Preconditions with sequences

When writing the precondition for a function that takes a sequence parameter, we must consider whether our function will only work correctly for particular sequence values or sizes. For example, our `makeFirstZero` function will not work if the size of the `seq` parameter is zero. We would need to add this requirement to the function's precondition:

```text
requires seq.size > 0
```

If we wanted to require that all values in a sequence parameter (say, `nums`) be between 10 and 20, we would write:

```text
requires ∀ x: (0..<nums.size) (nums(x) >= 10 ∧ nums(x) <= 20)
```

Sometimes, functions with sequence parameters will work for any size/values -- in those cases, we don't need to list anything about the sequence in the precondition.

## Function `modifies` clause

We learned in chapter 9 that the format of a function contract is:

```text
l"""{
    requires (preconditions)
    modifies (sequences/globals changed in this function)
    ensures (postconditions)
}"""
```

Until this point, we have been leaving off the `modifies` clause because our functions have not used sequences or global variables. We now need to include that clause whenever a function CHANGES the values in a sequence parameter. For example, the `makeFirstZero` function DOES change its sequence parameter, `seq`, as it sets its first position to 0. `makeFirstZero` should therefore include this `modifies` clause:

```text
modifies seq
```

If the function modifies more than one sequence or global variable, they are listed together in a comma-separated list.

## Postconditions with sequences

When writing the postcondition for a function that uses a sequence parameter, we must consider two things:

- How the return value relates to the sequence
- How the function will change the sequence

### Relating return values to sequence elements

We will still use the `result` keyword for describing the function's return value in the postcondition. For example, if a function was returning the smallest value in the sequence `nums`, we would say:

```text
ensures ∀ x: (0..<nums.size) result <= nums(x)
    ∃ x: (0..<a.size) result == nums(x)
```

Here, the first postcondition states that our return value will be less than or equal to every value in the sequence, and the second postcondition statees that our return value is one of the sequence elements. (The second postcondition prevents us from sneakily returning some large negative number and claiming that it was the smallest element in the sequence, when in fact it wasn't one of the sequence elements.)

Sometimes, our postconditions will promise to return a particular value if some claim about the sequence is true. Suppose we have a function that returns whether or not (i.e., a bool) all elements in the sequence `a` are negative. Our postcondition would be:

```text
ensures (∀ x: (0..<nums.size) nums(x) < 0) → (result == true)
    (∃ x: (0..<nums.size) nums(x) >= 0) → (result == false)
```

Here, the first postcondition promises that if all sequence elements are negative, then the function will return true. The second postcondition promises the opposite -- that if there is a nonnegative sequence element, then the function will return false.

### Describing how the function changes the sequence

Consider again the `makeFirstZero` function:

```text
def makeFirstZero(seq: ZS): Unit = {
    seq(0) = 0
}
```

This function doesn't return anything (hence the `Unit` return type), but we do need to describe what impact calling this function will have on the sequence. We can partially accomplish our goal with this postcondition:

```text
ensures seq(0) == 0
```

Which promises that after the function ends, the first value in the sequence will be 0. However, suppose we wrote this instead for the `makeFirstZero` function:

```text
def makeFirstZero(seq: ZS): Unit = {
    seq(0) = 0
    seq(1) = 100
}
```

This version of the function DOES satisfy the postcondition -- the first element is indeed set to 0 -- but it changes other elements, too. The postcondition should be descriptive enough that anyone calling it can be certain EXACTLY what every single value in the sequence will be afterwards. Our postcondition needs to describe exactly what values in the sequence WILL change and exactly what values WON'T change.

This means that our `makeFirstZero` function needs to state that the first element in `seq` gets set to 0, and that every other value in the sequence *stays the same as its original value*. To help us describe the *original value* of a sequence, we can use the special `sequenceName_in` syntax, which holds the value of a sequence parameter `sequenceName` at the time the function was called. (This `_in` syntax can only be used in logic blocks, not in the code.)

We can specify exactly what happens to each sequence element in our first version of `makeFirstZero` like this:

```text
ensures seq(0) == 0
    ∀ x: (1..<seq.size) seq(x) == seq_in(x)
```

The second postcondition says: "All elements from position 1 on keep their original values".

### Postcondition: size doesn't change

Logika has an oddity with programs that modify sequence parameters -- in those cases, you must also include as a postcondition that the size of the sequence will not change (i.e., that the resulting sequence size will equal the original sequence size. For example, if a function modified the sequence `seq`, we would need to add the postcondition:

```text
seq.size == seq_in.size
```

Logika is concerned that any function that modifies a sequence might also change the size of that sequence. While it is possible to append and prepend to Logika sequences (much like with Python lists), we cannot do so to sequence parameters. As a rule, Logika functions cannot assign to ANY parameter value (they are read-only). However, we still must state that the size doesn't change or the program will not be verified. Whenever you list a sequence in the `modifies` clause to a function, you must also include a postcondition to say that its size doesn't change.

If you are writing a function that uses a sequence parameter but doesn't change that parameter, you should not list that sequence in a `modifies` clause, and you should not state that the sequence size doesn't change (or anything else about the `..._in` value of the sequence).

## Example: finished `makeFirstZero` verification

Now that we have seen all the pieces of writing function contracts for functions that work with sequences, we can put together the full function contract for our `makeFirstZero` function. The assert statement in the test code will be verified in Logika's symexe mode:

```text
import org.sireum.logika._

//"Unit" is like a void return type
def makeFirstZero(seq: ZS): Unit = {
    l"""{
        requires seq.size >= 1  //we need at least 1 element
        modifies seq            //we are changing the sequence
        ensures
            //we promise the first element will be a 0
            seq(0) == 0

            //we promise every other element is the same as its original value
            A x: (1..<seq.size) seq(x) == seq_in(x)

            //we promise the sequence size won't change
            seq.size == seq_in.size
    }"""

    seq(0) = 0
}

///// Test code ///////////

var nums: ZS = ZS(1,2,3)
makeFirstZero(nums)

//we want to claim that nums is what it was, but with the first
//element as a 0
assert(nums == ZS(0,2,3))
```

## Example: swap program

Suppose we have the following swap program:

```text
import org.sireum.logika._

def swap(list: ZS, pos1: Z, pos2: Z): Unit = {
    var temp: Z = list(pos1)
    list(pos1) = list(pos2)
    list(pos2) = temp
}

///////////// Calling code ///////////////////

var testList: ZS = ZS(1,2,3,4)
swap(testList,0,3)

//the values in positions 0 and 3 should be swapped
//all other elements should be the same
assert(testList == ZS(4,2,3,1))
```

Here, `swap` takes an integer sequence (`list`) and two positions (`pos1` and `pos2`). It uses a temp variable to swap the values in `list` at `pos1` and `pos2`. We would like to write an appropriate function contract so the assert statement in the test code holds.

We must first consider the precondition -- does `swap` have any requirements about its parameters? Since `swap` uses `pos1` and `pos2` as positions within `list`, we can see that `swap` will crash if either position is out of bounds -- either negative or past the end of the sequence.

The function is changing the sequence, so we will need a `modifies` clause. Finally, we must consider the postcondition. This function isn't returning a value, but it is changing the sequence -- so we should describe exactly what values HAVE changed (and their new values) and what values have NOT changed. We want to say that:

- `list(pos1)` has the value that was originally at `list(pos2)` (i.e, the value at `list_in(pos2)`)
- `list(pos2)` has the value that was originally at `list(pos1)` (i.e, the value at `list_in(pos1)`)
- All other positions are unchanged (i.e., they are the same as they were in `list_in`)
- The size doesn't change (which we must always list if a sequence is modified)

We can now complete the function contract for `swap`:

```text
import org.sireum.logika._

def swap(list: ZS, pos1: Z, pos2: Z): Unit = {
    l"""{
        //pos1 and pos2 need to be valid positions
        requires pos1 >= 0
            pos2 >= 0
            pos1 < list.size
            pos2 < list.size
        modifies list
        ensures
            list(pos1) == list_in(pos2)
            list(pos2) == list_in(pos1)
            list.size == list_in.size

            //all the other spots stay the same
            A x:(0..<list.size) (x != pos1 ^ x != pos2) -> list(x) == list_in(x)
    }"""

    var temp: Z = list(pos1)
    list(pos1) = list(pos2)
    list(pos2) = temp
}

///////////// Calling code ///////////////////

var testList: ZS = ZS(1,2,3,4)
swap(testList,0,3)

//the values in positions 0 and 3 should be swapped
//all other elements should be the same
assert(testList == ZS(4,2,3,1))
```

If we test this program in Logika's symexe mode, the final assert will hold -- we have enough information to make a claim about EXACTLY what the sequence will look like after calling `swap`.