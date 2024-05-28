---
title: "Logika Programs"
pre: "8.2. "
weight: 91
date: 2018-08-24T10:53:26-05:00
---

As we study program logic, we will use Logika to verify a subset of programs that use the Scala language. Specifically, we will study verification of programs with the following features:

- Variables (booleans, ints, and sequences [which are like arrays/lists])
- Printing and user input
- Math operations
- Conditional operations
- If and if/else statements
- While loops
- Functions

The Scala programs we verify should be saved with a .sc extension. To show we are using Logika for verification, the first line of the file should be:

```text
// #Sireum #Logika
```

## Verifying Logika programs

There are two modes in Logika -- manual and automatic. In chapters 8 and 9, we will use manual mode. In chapter 10, we will use automatic mode. You will designate the verification mode on the second line of the Scala file (just below `// #Sireum #Logika`). To specify manual mode, you will write:

```text
//@Logika: --manual --background save
```

And to specify automatic mode, you will delete the `--manual` from above, like this:

```text
//@Logika: --background save
```

Logika verification *should* run automatically as you edit these programs and their proofs. If a program is verified, you will see a purple checkmark in the lower right corner just as you did in propositional and predicate logic proofs. If there are syntax or logic errors, you will see them highlighted in red.

Sometimes, the Logika verification needs to be run manually. If you don't see either red errors or a purple checkmark, right-click in the text area that contains the code and select "Logika Check (All in File)".

## Example programs

This section contains four sample Scala programs that highlight the different language features that we will work with.

### Example 1: User input, printing, operations

This first example gets a number as input from the user, adds one to it, and prints the result:

```text
// #Sireum #Logika
//@Logika: --manual --background save
import org.sireum._

var x: Z = Z.prompt("Enter a number: ")
x = x + 1

println("One more is ", x)
```

A few things to note:

- After the two comment lines to show we are using the Logika verification tool in manual mode, our program begins with an import statement: `import org.sireum._`
- The `var` keyword stands for variable, which is something that can be changed. Logika also has a `val` keyword, which creates a constant value that cannot be changed.
- Lines do not end in semi-colons
- The `Z.prompt(...)` function prints a prompt and returns the integer (`Z`) that was typed. The parameter to this function is the desired prompt. Alternately, the `Z.read()` function gets and returns an integer from user input without taking a prompt.
- The code `var x: Z` creates a variable called `x` of type `Z` -- as with the user input functions, the `Z` means *integer*

### Example 2: If/else statements

Here is a Scala program that gets a number from the user, and uses an if/else statement to print whether the number is positive or negative/zero:

```text
// #Sireum #Logika
//@Logika: --manual --background save
import org.sireum._

val num: Z = Z.prompt("Enter a number: ")

if (num > 0) {
    println(num, " is positive")
} else {
    println(num, " is negative (or zero)")
}
```

A couple of things to note here:

- The `else` statement MUST appear on the same line as the closing `}` of the previous if-statement
- As mentioned above, the `val` keyword in this program means that `num` cannot be changed after being initialized

### Example 3: While loops

Our next program uses a while loop to print the numbers from 10 down to 1:

```text
// #Sireum #Logika
//@Logika: --manual --background save
import org.sireum._

var cur: Z = 10
while (cur >= 1) {
  println("Next number: ", cur)
  cur = cur - 1
}
```

### Example 4: Sequences and functions

Our final sample  program demonstrates *sequences* (similar to an array or list) and functions. It contains a function, `sumSequence`, which takes a sequence of integers as a parameter and returns the sum of the numbers in the sequence. At the bottom, we can see our test code that creates a sample sequence and tries calling `sumSequence`. If you create this file, you will see an error about the sequence indices possibly being negative -- this code cannot be run without some verification work:

```text
// #Sireum #Logika
//@Logika: --manual --background save
import org.sireum._

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

- These functions are not part of a class - they are more similar to the structure in Python. We can include as many functions as we want in a file. At the bottom of the file (marked below the optional `////// Calling code ////////////`) is the *calling code*. When a Logika program runs, those calling code lines (which may call different functions) are executed. When the calling code lines are done, the program is done.

## Logika program proof syntax

In order to prove correctness of these Scala programs, we will add *Deduce blocks* to process what we have learned at different points in the program. In general, every time there is an assignment statement, there will be a following `Deduce` proof block updating all relevant facts.

### Necessary import statements

We have already seen that we must include the import statement:

```text
import org.sireum._
```

Anytime we want to do anything with the Logika proof checker. There are three other import statements that you will commonly use:

1. `import org.sireum.justification._` is needed for any Deduce statements and basic justifications (`Premise`, etc.)
2. `import org.sireum.justification.natded.prop._` is needed for any propositional logic justifications
3. `import org.sireum.justification.natded.pred._` is needed for any predicate logic justifications

### Deduce block

Here is the syntax for a Deduce proof block:

```text
Deduce(
    1  (    claim       ) by Justification,
    ...
)
```

Just as with our propositional and predicate logic proofs, we will number the lines in our proof blocks. Each line will contain a *claim* and a corresponding *justification* for that claim. We will still be able to use all of our propositional and predicate logic deduction rules, but we will learn new justifications for processing program statements. Each program may have multiple of these proof blocks to process each assignment statement.

### Premise justification

Our first justification in programming logic is *Premise*. In a Deduce proof block, we use `Premise` as a justification in the following cases:

- To express an assignment statement (or other program statement)

- To pull a claim established in a previous proof block into a later proof block

In both cases, the claim must capture the current value of the involved variables.

For example, consider the following program:

```text
// #Sireum #Logika
//@Logika: --manual --background save
import org.sireum._
import org.sireum.justification._

var x: Z = 6
x = x + 1
```

We could insert a proof block between the two lines to express that `x` currently has the value 6:

```text
// #Sireum #Logika
//@Logika: --manual --background save
import org.sireum._
import org.sireum.justification._

var x: Z = 6

Deduce(
  1 (   x == 6  ) by Premise
)

x = x + 1
```

But we could NOT have the same proof block after incrementing `x`, since `x`'s value has changed since that claim was established:

```text
// #Sireum #Logika
//@Logika: --manual --background save
import org.sireum._
import org.sireum.justification._

var x: Z = 6

//this proof block is correct -- it captures the current value of x
Deduce(
  1 (   x == 6  ) by Premise
)

x = x + 1

//NO! This statement no longer captures the current value of x
Deduce(
  1 (   x == 6  ) by Premise
)

```

### Using previous deduction rules

We can use any of our previous deduction rules in a Logika proof block. For example:

```text
// #Sireum #Logika
//@Logika: --manual --background save
import org.sireum._
import org.sireum.justification._
import org.sireum.justification.natded.prop._

val x: Z = 6
val y: Z = 7

Deduce(
  1 (   x == 6                ) by Premise,
  2 (   y == 7                ) by Premise,
  3 (   (x == 6) ∧ (y == 7)   ) by AndI(1, 2)
)
```