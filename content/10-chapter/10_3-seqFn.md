---
title: "Sequences in Functions"
pre: "10.3 "
weight: 112
date: 2018-08-24T10:53:26-05:00
---

Sequences in Scala are passed to functions by reference. This means that if a function makes changes to a sequence parameter, then it will also change whatever sequence object was passed to the function. For example, the test code below passes `nums`, which has the value `ZS(1,2,3)`, to the `makeFirstZero` function. The `makeFirstZero` function changes the first position in its parameter (`seq`) to be 0, which means that the `nums` sequence in the test code will also have its first position set to 0 (making it have the value `ZS(0,2,3)`).

```text
// #Sireum #Logika
//@Logika: --background save
import org.sireum._

def makeFirstZero(seq: ZS): Unit = {  // Unit is like void in Java and C#
    seq(0) = 0
}

///////////// Test code /////////////////

var nums: ZS = ZS(1,2,3)
makeFirstZero(nums)

assert(nums == ZS(0,2,3))
```

## Preconditions with sequences

When writing the precondition for a function that takes a sequence parameter, we must consider whether our function will only work correctly for particular sequence values or sizes. For example, our `makeFirstZero` function will not work if the size of the `seq` parameter is zero. We would need to add this requirement to the function's precondition (to its `Requires` clause):

```text
seq.size > 0
```

If we wanted to require that all values in a sequence parameter (say, `nums`) be between 10 and 20, we would add the following to the precondition:

```text
∀ (0 until nums.size)(x => (nums(x) >= 10 ∧ nums(x) <= 20))

```

Sometimes, functions with sequence parameters will work for any size/values -- in those cases, we don't need to list anything about the sequence in the precondition.

## Function `Modifies` clause

We learned in chapter 9 that the format of a function contract is:

```text
Contract(
    Requires (  preconditions   ),
    Ensures (   postconditions  )
)
```

However, there is actually a third portion of the function contract that we have omitted until this point -- the `Modifies` clause. This clause is required whenever a function changes the values in a sequence parameter or changes global variables. For example, the `makeFirstZero` function DOES change its sequence parameter, `seq`, as it sets its first position to 0. `makeFirstZero` should therefore include this `Modifies` clause:

```text
Modifies ( seq ),
```

This clause goes between the precondition (`Requires`) and the postcondition (`Ensures`), making the new template for the function contract look like this:

```text
Contract(
    Requires (  preconditions   ),
    Modifies (  comma-separated list of sequences/global variables modified by the function ),
    Ensures (   postconditions  )
)
```

If the function modifies more than one sequence or global variable, they are listed together in a comma-separated list in the `Modifies` clause.

## Postconditions with sequences

When writing the postcondition for a function that uses a sequence parameter, we must consider two things:

- How the return value relates to the sequence
- How the function will change the sequence

### Relating return values to sequence elements

We will still use the `Res[Type]` keyword for describing the function's return value in the postcondition. For example, if a function was returning the smallest value in the integer sequence `nums`, we would say:

```text
Ensures(
    ∀ (0 until nums.size)(x => (Res[Z] <= nums(x))),
    ∃ (0 until nums.size)(x => (Res[Z] == nums(x)))
)
```

Here, the first postcondition states that our return value will be less than or equal to every value in the sequence, and the second postcondition states that our return value is one of the sequence elements. (The second postcondition prevents us from sneakily returning some negative number and claiming that it was the smallest element in the sequence, when in fact it wasn't one of the sequence elements.)

Sometimes, our postconditions will promise to return a particular value if some claim about the sequence is true. Suppose we have a function that returns whether or not (i.e., a boolean) all elements in the sequence `a` are negative. Our postcondition would be:

```text
Ensures(
    ( ∀ (0 until a.size)(x => (a(x) < 0)) ) → (result == true),
    ( ∃ (0 until a.size)(x => (a(x) >= 0)) ) → (result == false),
)
```

Here, the first postcondition promises that if all sequence elements are negative, then the function will return true. The second postcondition promises the opposite -- that if there is a nonnegative sequence element, then the function will return false.

### Describing how the function changes the sequence

Consider again the `makeFirstZero` function:

```text
def makeFirstZero(seq: ZS): Unit = {
    seq(0) = 0
}
```

This function doesn't return anything (hence the `Unit` return type), but we do need to describe what impact calling this function will have on the sequence. We can partially accomplish our goal with this postcondition (in our `Ensures` clause):

```text
seq(0) == 0
```

Which promises that after the function ends, the first value in the sequence will be 0. However, suppose we wrote this instead for the `makeFirstZero` function:

```text
def makeFirstZero(seq: ZS): Unit = {
    seq(0) = 0
    seq(1) = 100
}
```

This version of the function DOES satisfy the postcondition -- the first element is indeed set to 0 -- but it changes other elements, too. The postcondition should be descriptive enough that anyone calling it can be certain EXACTLY what every single value in the sequence will be afterwards. Our postcondition needs to describe exactly what values in the sequence WILL change and exactly what values WON'T change.

This means that our `makeFirstZero` function needs to state that the first element in `seq` gets set to 0, and that every other value in the sequence *stays the same as its original value*. To help us describe the *original value* of a sequence, we can use the special `In(sequenceName)` syntax, which holds the value of a sequence parameter `sequenceName` at the time the function was called. (This `In` syntax can only be used in logic blocks, not in the code.)

We can specify exactly what happens to each sequence element in our first version of `makeFirstZero` like this:

```text
Ensures(
    seq(0) == 0,
    ∀ (1 until seq.size)(x => ( seq(x) == In(seq)(x)))
)
```

The second postcondition says: "All elements from position 1 on keep their original values".

## Example: finished `makeFirstZero` verification

Now that we have seen all the pieces of writing function contracts for functions that work with sequences, we can put together the full function contract for our `makeFirstZero` function. The assert statement in the test code will be verified by Logika's auto mode:

```text
// #Sireum #Logika
//@Logika: --background save
import org.sireum._

def makeFirstZero(seq: ZS): Unit = {
    Contract(
        Requires(  seq.size > 0  ),
        Modifies(  seq  ), 
        Ensures(
        seq(0) == 0,
        ∀ (1 until seq.size)(x => ( seq(x) == In(seq)(x)))
        )
    )

    seq(0) = 0
}

//////////////// Test code //////////////

var nums: ZS = ZS(1,2,3)
makeFirstZero(nums)

assert(nums == ZS(0,2,3))
```

## Example: swap program

Suppose we have the following swap program:

```text
// #Sireum #Logika
//@Logika: --background save
import org.sireum._

def swap(list: ZS, pos1: Z, pos2: Z): Unit = {
    val temp: Z = list(pos1)
    list(pos1) = list(pos2)
    list(pos2) = temp
}

///////////// Test code ///////////////////

var testList: ZS = ZS(1,2,3,4)
swap(testList,0,3)

//the values in positions 0 and 3 should be swapped
//all other elements should be the same
assert(testList == ZS(4,2,3,1))
```

Here, `swap` takes an integer sequence (`list`) and two positions (`pos1` and `pos2`). It uses a temp variable to swap the values in `list` at `pos1` and `pos2`. We would like to write an appropriate function contract so the assert statement in the test code holds.

We must first consider the precondition -- does `swap` have any requirements about its parameters? Since `swap` uses `pos1` and `pos2` as positions within `list`, we can see that `swap` will crash if either position is out of bounds -- either negative or past the end of the sequence.

The function is changing the sequence, so we will need a `Modifies` clause. Finally, we must consider the postcondition. This function isn't returning a value, but it is changing the sequence -- so we should describe exactly what values HAVE changed (and their new values) and what values have NOT changed. We want to say that:

- `list(pos1)` has the value that was originally at `list(pos2)` (i.e, the value at `In(list)(pos2)`)
- `list(pos2)` has the value that was originally at `list(pos1)` (i.e, the value at `In(list)(pos1)`)
- All other positions are unchanged (i.e., they are the same as they were in `In(list)`)
- The size doesn't change (which we must always list if a sequence is modified)

We can now complete the function contract for `swap`:

```text
// #Sireum #Logika
//@Logika: --background save
import org.sireum._

def swap(list: ZS, pos1: Z, pos2: Z): Unit = {
    Contract(
        Requires(
            0 <= pos1 & pos1 < list.size, // pos1 is a valid index
            0 <= pos2 & pos2 < list.size  // pos2 is a valid index
        ),
        Modifies(list),           // documents list is modified
        Ensures(
            list(pos1) == In(list)(pos2),
            list(pos2) == In(list)(pos1),
            list.size == In(list).size,
            ∀ (0 until list.size)(x => (x != pos1 & x != pos2 → list(x) == In(list)(x)))
        )
  )

  val temp: Z = list(pos1)
  list(pos1) = list(pos2)
  list(pos2) = temp
}

//////////////// Test code //////////////

var testList: ZS = ZS(1,2,3,4)
swap(testList,0,3)

assert(testList == ZS(4,2,3,1))
```

If we test this program in Logika's symexe mode, the final assert will hold -- we have enough information to make a claim about EXACTLY what the sequence will look like after calling `swap`.

Note that we could have simplified the postcondition by using the *sequence update notation* described at the end of section 10.2. We want to say that the value of `list` at the end of the function is equivalent to the value of list at the beginning of the function (`In(list)`) with the following exceptions:

- The new value at `pos1` should be the original value at `pos2` (`In(list)(pos2)`)
- The new value at `pos2` should be the original value at `pos1` (`In(list)(pos1)`)

Here is the equivalent (but shorter) postcondition for `swap`:

```text
Ensures(
    list ≡ In(list)(pos1 ~> In(list)(pos2), pos2 ~> In(list)(pos1))
)
```