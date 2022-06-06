---
title: "Logika Programs"
pre: "8.2. "
weight: 91
date: 2018-08-24T10:53:26-05:00
---

As we study program logic, we will use a toy language within Logika. These Logika programs use a subset of the Scala language, and include the following features:

- Variables (booleans, ints, and sequences [which are like arrays/lists])
- Printing and user input
- Math operations
- Conditional operations
- If and if/else statements
- While loops
- Functions

## Running Logika programs

Logika program should be saved with a .logika extension. To run a Logika program, right-click in the text area that contains the code and select "Run Logika Program".

## Verifying Logika programs

There are two modes in Logika -- manual and SymExe. In chapters 8 and 9, we will use manual mode. In chapter 10, we will use SymExe mode. You can change the mode in Logika by going to File->Settings->Tools->Sireum->Logika.

For manual mode, uncheck "Auto mode" and mark the Checker kind as "Forward." 

For SymExe mode, check "Auto mode" and mark the Checker kind as "Summarizing SymExe".

## Example programs

This section contains four sample Logika programs that highlight the different language features.

### Example 1: User input, printing, operations

This first example gets a number as input from the user, adds one to it, and prints the result:

```text
import org.sireum.logika._

var x: Z = readInt("Enter a number: ")
x = x + 1
println("One more is ", x)
```

A few things to note:

- Each Logika program begins with an import statement: `import org.sireum.logika._`
- The `var` keyword stands for variable, which is something that can be changed. Logika also has a `val` keyword, which creates a constant value that cannot be changed.
- Lines do not end in semi-colons
- The parameter to the `readInt(...)` function call is a prompt telling the user what to type. This parameter is optional.
- The code `var x: Z` creates a variable called `x` of type `Z` -- the `Z` means *integer*

### Example 2: If/else statements

Here is a Logika program that gets a number from the user, and uses an if/else statement to print whether the number is positive or negative/zero:

```text

import org.sireum.logika._

val num: Z = readInt("Enter a number: ")

if (num > 0) {
    println(num, " is positive")
} else {
    println(num, " is negative (or zero)")
}
```

A couple of things to note here:

- The `else` statement MUST appear on the same line as the closing `}` of the previous if-statement
- As mentioned above, the `val` keyword in this program means that `x` cannot be changed after being initialized

### Example 3: While loops

Our next Logika program uses a while loop to print the numbers from 10 down to 1. This program can't be run the way it appears below, as Logika wants you to do some verification work first. However, this program demonstrates what a while loop will look like:

```text
import org.sireum.logika._

var cur: Z = 10
while (cur >= 1) {
    println("Next number: ", cur)
    cur = cur - 1
}
```

### Example 4: Sequences and functions

Our final sample Logika program demonstrates *sequences* (Logika's version of an array or list) and functions. It contains a function, `sumSequence`, which takes a sequence of integers as a parameter and returns the sum of the numbers in the sequence. At the bottom, we can see our test code that creates a sample sequence and tries calling `sumSequence`. As in our third example, this code cannot be run without some verification work:

```text
import org.sireum.logika._

def sumSequence(seq: ZS) : Z = {
    var sum: Z = 0

    var i: Z = 0

    while (i < seq.size) {
        sum = sum + seq(i)
        i = i + 1
    }

    return sum
}

////// Calling code ////////////

val list: ZS = ZS(1,2,3,4)
val total: Z = sumSequence(list)
println("Sum of elements:", total)
```

A few things of note here:

- The definition `def sumSequence(seq: ZS) : Z` means that a function named `sumSequence` takes a parameter of type `ZS` (sequence of integers, `Z` = int and `S` = sequence) and returns something of type `Z` (int)

- There is an `=` after the function header but before the opening `{` of the function

- Functions in Logika are not part of a class - they are more similar to the structure in Python. We can include as many functions as we want in a file. At the bottom of the file (marked below the optional `////// Calling code ////////////`) is the *calling code*. When a Logika program runs, those calling code lines (which may call different functions) are executed. When the calling code lines are done, the program is done.

## Logika program proof syntax

In order to prove correctness of Logika programs, we will add *Logika proof blocks* to process what we have learned at different points in the program. In general, every time there is an assignment statement, there will be a following Logika proof block updating all relevant facts.

### Proof block

Here is the syntax for a proof block:

```text
l"""{
    lineNumber. claim               justification
    ...
}"""
```

Just as with our propositional and predicate logic proofs, we will number the lines in our proof blocks. Each line will contain a *claim* and a corresponding *justification* for that claim. We will still be able to use all of our propositional and predicate logic deduction rules, but we will learn new justifications for processing program statements. Each program may have multiple of these proof blocks to process each assignment statement.

### Premise justification

Our first justification in programming logic is *premise*. In a Logika proof block, we use premise as a justification in the following cases:

- To express an assignment statement (or other program statement)

- To pull a claim established in a previous proof block into a later proof block

In both cases, the claim must capture the current value of the involved variables.

For example, consider the following program:

```text
import org.sireum.logika._

val x: Z = 6
x = x + 1
```

We could insert a proof block between the two lines to express that `x` currently has the value 6:

```text
import org.sireum.logika._

val x: Z = 6

l"""{
    1. x == 6               premise
}"""

x = x + 1
```

But we could NOT have the same proof block after incrementing `x`, since `x`'s value has changed since that claim was established:

```text
import org.sireum.logika._

val x: Z = 6

//this proof block is correct -- it captures the current value of x
l"""{
    1. x == 6               premise
}"""

x = x + 1

//NO! This statement no longer captures the current value of x
l"""{
    1. x == 6               premise
}"""
```

### Using previous deduction rules

We can use any of our previous deduction rules in a Logika proof block. For example:

```text
import org.sireum.logika._

val x: Z = 6
val y: Z = 7

l"""{
    1. x == 6               premise
    2. y == 7               premise
    3. (x == 6) ∧ (y == 7)  ∧i 1 2
}"""
```