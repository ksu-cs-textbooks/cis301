---
title: "Sequences in Loops"
pre: "10.4 "
weight: 113
date: 2018-08-24T10:53:26-05:00
---

We also must consider sequences when writing loop invariants. Typically, we must include the following in our invariant:

- If the sequence changes in the loop
    - Describe what sequence elements have already changed in the loop (and what their new values are)
    - Describe what sequence elements still have their original value
    - Prove lower and upper bounds for whatever variable is being used as a sequence position (so we can be certain we will not go past the bounds of the sequence)
    - State that the sequence size does not change (to reinforce that the variable used as the sequence position will not go out of bounds)
    - List the sequence along with other changing variables in the loop invariant block's `Modifies` clause
- If the sequence does not change in the loop
    - Consider what we are doing with each sequence element as we look at them. Usually we have another variable that is storing our progress (and often, this variable is returned from the function after the loop). Express how the variable's value relates to the part of the sequence we've looked at so far -- this statement should look very similar to your postcondition, but should only describe part of the sequence.
    - Prove lower and upper bounds for whatever variable is being used as a sequence position (so we can be certain we will not go past the bounds of the sequence)

## Example: add one to all program

Suppose we have the following program, which adds one to every element in a sequence parameter:

```text
// #Sireum #Logika
//@Logika: --background save
import org.sireum._

def addOne(list: ZS): Unit = {
    var i: Z = 0
    while (i < list.size) {
        list(i) = list(i) + 1
        i = i + 1
    }
}

////////////// Calling code ///////////////////

var test: ZS = ZS(1,2,3,4)
addOne(test)

assert(test == ZS(2,3,4,5))
```

We would like to write an appropriate function contract and loop invariant block so the assert statement in the test code holds (which asserts that the sequence `ZS(1,2,3,4)` becomes the sequence `ZS(2,3,4,5)` after calling the function).

For the function contract, we must consider:

- *Precondition*: this function will work correctly on all sequences -- even empty ones. We can leave the `Requires` clause off.
- `Modifies` clause: this function is changing the `list` sequence parameter, so we must list it in a `modifies` clause.
- *Postcondition*: the function is not returning anything, but we must describe that all sequence parameters will be one bigger than their original values.

For the loop invariant block, we notice that the loop is changing the sequence. We must include:

- *Which elements have already changed*. Since `i` is tracking our position in the sequence, we know that at the end of each iteration, all elements from position 0 up to but not including position `i` have been changed to be one bigger than their original values.
- *Which elements have not changed*. All other elements in the sequence -- from position `i` to the end of the sequence -- still have their original values
- *Upper and lower bounds for position variables*. Since `i` is tracking our position, we must state that i is always a valid sequence index. Here, we need to claim that `i` will always be greater than or equal to 0 and less than or equal to the sequence size. (While the sequence size itself is not a valid sequence index, we see from looking at the loop that `i` is incrementing as the very last step in the loop. On the last iteration, `i` will start off at `list.size-1`, and we will correctly access and modify the last element in `list`. Then we will increment `i`, making it EQUAL `list.size` -- at that point, the loop ends. If we made part of our invariant be `i < list.size`, it would be incorrect because of that last iteration.)
- *State that the sequence size does not change*. This is necessary any time we are writing a loop modifying a sequence in order to ensure that the sequence index (`i` in this case) will not go past the end of the sequence.

We can now complete the function contract and loop invariant for `addOne`:

```text
// #Sireum #Logika
//@Logika: --background save
import org.sireum._

def addOne(list: ZS): Unit = {
    Contract(
        Modifies(list),
        Ensures(
            ∀ (0 until list.size)(x => (list(x) == In(list)(x) + 1))
        )
    )

    var i: Z = 0
    while (i < list.size) {
        Invariant(
            Modifies(i, list),
            i >= 0,
            i <= list.size,

             //the sequence size never changes
            list.size == In(list).size,

             //what I HAVE changed
            ∀ (0 until i)(x => (list(x) == In(list)(x) + 1)),

             //what I HAVEN'T changed
            ∀ (i until list.size)(x => (list(x) == In(list)(x)))
        )


        list(i) = list(i) + 1
        i = i + 1
    }
}

////////////// Calling code ///////////////////

var test: ZS = ZS(1,2,3,4)
addOne(test)

assert(test == ZS(2,3,4,5))

```

If we try to verify this program in Logika's auto mode, the final assert will hold -- we have enough information to know what the sequence will look like after calling `addOne`.

## Example: min program

In our next example, we examine a function that does *not* modify its sequence parameter, and that *does* return a value. Consider the following `min` function and test code:

```text
// #Sireum #Logika
//@Logika: --background save
import org.sireum._


//return the smallest element in list
def min(list: ZS): Z = {
    var small: Z = list(0)
    var i: Z = 1

    while (i < list.size) {
        if (list(i) < small) {
            small = list(i)
        }

        i = i + 1
    }

    return small
}

////////////// Calling code ///////////////////

var test: ZS = ZS(8,1,0,10,9,2,0)
var testMin: Z = min(test)

assert(testMin == 0)
```

Here, our `min` function is supposed to find and return the smallest value in an integer sequence. We can see that our test code passes `min` the sequence (`ZS(8,1,0,10,9,2,0)`), and that we are trying to assert that `min` correctly returns `0` as the smallest value in the sequence. We need to add an appropriate function contract and loop invariant block to make this assert hold.

For the function contract, we must consider:

- *Precondition*: this function starts by saving out the element at position 0. If the sequence was empty, the function would crash. We need to require that the sequence size be at least 1.
- `Modifies` clause: this function is NOT modifying its sequence parameter, so we can omit this clause
- *Postcondition*: the function is not changing the sequence, so we do not need to describe the final values of each sequence element. We *do* need to describe what value we are returning, and how it relates to the sequence. We want to describe that `Res[Z]` (our return value) is the smallest integer in `list`, so that:
    - `Res[Z]` is less than or equal to every element in `list`
    - There is an element in `list` that equals `Res[Z]` (i.e., we really are returning one of our sequence values)

For the loop invariant block, we notice that the loop is NOT changing the sequence. We must include:

- *What we are doing with each sequence element, and how that relates to another variable*. We can see that `small` tracks the smallest element we've seen in the sequence so far -- up to but not including position `i`. Similar to the postcondition, we want to claim that:
    - `small` is less than or equal to every element in `list` *that we have seen so far*
    - There is an element *we have already seen* in `list` that equals small
 - *Upper and lower bounds for position variables*. Here, `i` is our position variable. We see that it is initialized to 1, so we will claim that it is always greater than or equal to 1 and less than or equal to the list size.
 - `Modifies` clause - the variables `small` and `i` are modified in the while loop

 We can now complete the function contract and loop invariant for `min`:

```text
// #Sireum #Logika
//@Logika: --background save
import org.sireum._

def min(list: ZS): Z = {
    Contract(
        Requires (list.size >= 1),
        Ensures(
            ∀ (0 until list.size)(x => (Res[Z] <= list(x))),
            ∃ (0 until list.size)(x => (Res[Z] == list(x)))
        )
    )

    var small: Z = list(0)
    var i: Z = 1

    while (i < list.size) {
        Invariant(
            Modifies(i, small),
            i >= 1,
            i <= list.size,
            ∀ (0 until i)(x => (small <= list(x))),
            ∃ (0 until i)(x => (small == list(x)))
        )

        if (list(i) < small) {
            small = list(i)
        }

        i = i + 1
    }

    return small
}

////////////// Calling code ///////////////////

var test: ZS = ZS(8,1,0,10,9,2,0)
var testMin: Z = min(test)

assert(testMin == 0)
```

If we try to verify this program in Logika's auto mode, the final assert will hold -- we have enough information to know exactly the minimum value in `test`.