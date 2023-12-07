---
title: "Global Variables"
pre: "10.6 "
weight: 115
date: 2018-08-24T10:53:26-05:00
---

## Motivation

We will now consider programs with multiple functions that modify a shared pool of *global variables*. (This is very similar to the concerns in general classes in Java or C#, where multiple methods might edit fields/property values for an object). We want to be sure that global variables will maintain desired ranges and relationships between one another, even as multiple functions modify their values. 

## Global variables in Logika

A global variable in Logika exists before any function call, and still exists after any function ends. 

### Functions that access global variables

Consider the following Logika program:

```text
import org.sireum.logika._

//global variable
var feetPerMile: Z = 5280  // feet in a mile mile

def convertToFeet(m : Z): Z = {
    val feet: Z = m * feetPerMile
    return feet
}

/////////// Calling code ////////////////////

var miles: Z = readInt()

var totalFeet: Z = 0
if (miles >= 0){
    totalFeet = convertToFeet(miles)
}
```

Here, `feetPerMile` is a global variable -- it exists before the `convertToFeet` function is called, and still exists after `convertToFeet` ends. In contrast, the `feet` variable inside `convertToFeet` is NOT global -- its scope ends when the `convertToFeet` function returns.

(The `miles` and `totalFeet` variables in the calling code do not behave as global variables, as they were declared after any function definition. However, if we did add additional functions after our calling code, then `miles` and `totalFeet` would be global to those later functions. In Logika, the scope for any variable declared outside of a function begins at the point in the code where it is declared.)

In the example above, `convertToFeet` only accesses the `feetPerMile` global variable. A global variable that is read (but not updated) by a function body can be safely used in the functions precondition and postcondition -- it acts just like an extra parameter to the function. We might edit `convertToFeet` to have this function contract:

```text
import org.sireum.logika._

//global variable
var feetPerMile: Z = 5280  // feet in a mile mile

def convertToFeet(m : Z): Z = {
    l"""{
        //only do conversions on nonnegative distances
        requires m >= 0    
            //not needed, but demonstrates using global variables in preconditions     
            feetPerMile > 5200  

        //can use global variable in postcondition    
        ensures result == m * feetPerMile
    }"""

    val feet: Z = m * feetPerMile
    return feet
}

/////////// Calling code ////////////////////

var miles: Z = readInt()

var totalFeet: Z = 0
if (miles >= 0){
    totalFeet = convertToFeet(miles)
}
```

However, we cannot assign to a global variable the result of calling a function. That is, `totalFeet = convertToFeet(5)` is ok, and so is `totalFeet = convertToFeet(feetPerMile)`, but `feetPerMile = convertToFeet(5)` is not.

### Functions that modify global variables

In the Logika language, every global variable that is modified by a function must be listed in that function's `modifies` clause. Such functions must also describe in their postconditions how these global variables will be changed by the function from their original (pre-function call) values. We will use the notation `globalVariableName_in` for the value of global variable `globalVariableName` at the start of the function, just as we did for sequences.

Here is an example:

```text
import org.sireum.logika._

//global variable
var time: Z = 0

def tick(): Z = {
    l"""{
        requires time > 0
        modifies time
        ensures time == time_in + 1
    }"""

    time = time + 1
}
```

Here, we have a global `time` variable and a `tick` function that increases the time by 1 with each function call. Since the `tick` function changes the `time` global variable, we must include two things in its function contract:

- A `modifies` clause that lists `time` as one of the global variables modified by this function
- A postcondition that describes how the value of `time` after the function call compares to the value of `time` just before the function call. The statement `time == time_in + 1` means: "the value of time after the function call equals the value of time just before the function call, plus one".

## Global invariants

When we have a program with global variables that are modified by multiple functions, we often want some way to ensure that the global variables always stay within a desired range, or always maintain a particular relationship among each other. We can accomplish these goals with *global invariants*, which specify what must always be true about global variables.

### Bank example

For example, consider the following partial program that represents a bank account:

```text
import org.sireum.logika._

//global variables
var balance: Z = 0
var elite: B = false
val eliteMin: Z = 1000000 //$1M is the minimum balance for elite status

//global invariants
l"""{
    invariant
        //balance should be non-negative
        balance >= 0

        //elite status should reflect if balance is at least a million
        elite == (balance >= eliteMin)
}"""

def deposit(amount: Z): Unit = {
    l"""{
        //We still need to complete the function contract
    }"""

    balance = balance + amount
    if (balance >= eliteMin) {
        elite = true
    } else {
        elite = false
    }
}

def withdraw(amount: Z): Unit = {
    l"""{
        //We still need to complete the function contract
    }"""

    balance = balance - amount
    if (balance >= eliteMin) {
        elite = true
    } else {
        elite = false
    }
}
```

Here, we have three global variables: `balance` (the bank account balance), `elite` (whether or not the customer has "elite status" with the bank, which is given to customers maintaining above a certain balance threshold), and `eliteMin` (a value representing the minimum account balance to achieve elite status). We have two global invariants describing what must always be true about these global variables:

- `balance >= 0`, which states that the account balance must never be negative
- `elite == (balance >= eliteMin)`, which states that the `elite` boolean flag should always accurately represent whether the customer's current account balance is over the minimum threshold for elite status

### Global invariants must hold before each function call

In any program with global invariants, we either must prove (in manual mode) or their must be sufficient evidence (in symexe mode) that each global invariant holds immediately before any function call (including when the program first begins, before any function call). In our bank example, we see that the global variables are initialized as follows:

```text
var balance: Z = 0
var elite: B = false
val eliteMin: Z = 1000000
```

In symexe mode, there is clearly enough evidence that the global invariants all hold with those initial values -- the balance is nonnegative, and the customer correctly does not have elite status (because they do not have about the $1,000,000 threshold).

### Global invariants must still hold at the end of each function call

Since we must demonstrate that global invariants hold before each function call, functions themselves can assume the global invariants are true at the beginning of the function. If we were using manual mode, we could list each global invariant as a `premise` at the beginning of the function -- much like we do with preconditions. Then, it is the job of each function to ensure that the global invariants STILL hold when the function ends. In manual mode, we would need to demonstrate that each global invariant claim `globalInvariant` still held in a logic block just before the end of the function:

```text
l"""{
    //each global invariant must still hold at the end of the function

    1. globalInvariant              (some justification)
}"""
```

In symexe mode, we do not need to include such logic blocks, but there must be sufficient detail in the function contract to infer that each global invariant will hold no matter what at the end of the function.

### Bank function contracts

Consider the `deposit` function in our bank example:

```text
def deposit(amount: Z): Unit = {
    l"""{
        //We still need to complete the function contract
    }"""

    balance = balance + amount
    if (balance >= eliteMin) {
        elite = true
    } else {
        elite = false
    }
}
```

Since `deposit` is modifying the global variables `balance` and `elite`, we know we must include two things in its function contract:

- A `modifies` clause that lists `balance` and `elite` as global variables modified by this function
- A postcondition that describes how the value of `balance` after the function call compares to the value of `balance` just before the function call. We want to say, `balance == balance_in + amount`, because the value of `balance` at the end of the function equals the value of `balance` at the beginning of the function, plus `amount`.

We also must consider how the `elite` variable changes as a result of the function call. In the code, we use an if/else statement to ensure that `elite` gets correctly updated if the customer's new balance is above or below the threshold for elite status. If we were to write a postcondition that summarized how `elite` was updated by the function, we would write: `elite == (balance >= eliteMin)` to say that the value of elite after the function equaled whether the new balance was above the threshold. However, this claim is already a global invariant, which already must hold at the end of the function. We do not need to list it again as a postcondition.

Consider this potential function contract for `deposit`:

```text
def deposit(amount: Z): Unit = {
    l"""{
        //this function contract is not quite correct

        modifies balance, elite
        ensures balance == balance_in + amount
    }"""

    balance = balance + amount
    if (balance >= eliteMin) {
        elite = true
    } else {
        elite = false
    }
}
```

This function contract is close to correct, but contains a major flaw. In symexe mode, the function contract must be tight enough to guarantee that the global invariants will still hold after the function ends. Suppose `balance` still has its starting value of 0, and that we called `deposit(-100)`. With no other changes, the function code would dutifully update the `balance` global variable to be -100...which would violate the global invariant that `balance >= 0`. In order to guarantee that the balance will never be negative after the `deposit` function ends, we must restrict the deposit amounts to be greater than or equal to 0. Since functions are can assume that the global invariants hold when they are called, we know that `balance` will be 0 at minimum at the beginning of `deposit`. If `amount` is also nonnegative, we can guarantee that the value of `balance` at the end of the `deposit` function will be greater than or equal to 0 -- thus satisfying our global invariant.

Here is the corrected `deposit` function:

```text
def deposit(amount: Z): Unit = {
    l"""{
        requires amount >= 0
        modifies balance, elite
        ensures balance == balance_in + amount
    }"""

    balance = balance + amount
    if (balance >= eliteMin) {
        elite = true
    } else {
        elite = false
    }
}
```

We can similarly write the function contract for the `withdraw` function. Since withdraw is subtracting an amount from the balance, we must require that the withdraw amount be less than or equal to the account balance -- otherwise, the account balance might become negative, and we would violate the global invariant. We will also require that our withdrawal amount be nonnegative, as it doesn't make any sense to withdraw a negative amount from a bank account:

```text
def withdraw(amount: Z): Unit = {
    l"""{
        requires amount >= 0
            amount <= balance
        modifies balance, elite
        ensures
            balance == balance_in - amount
    }"""

    balance = balance - amount
    if (balance >= eliteMin) {
        elite = true
    } else {
        elite = false
    }
}
```

### Bank calling code

When we call a function in a program with global invariants (whether in the calling code or from another function), we must consider four things:

- We must demonstrate that all global variables hold before the function call
- We must demonstrate that the preconditions for the function holds
- We can assume that all global variables hold after the function call (as the function itself if responsible for showing that the global invariants still hold just before the function ends)
- We can assume the postcondition for the function holds after the function call

Suppose we had this test code at the end of our bank program:

```text
deposit(500000)

//Assert will hold
assert(balance == 500000 & elite == false)

deposit(500000)

//Assert will hold
assert(balance == 1000000 & elite == true)

//Precondition will not hold
withdraw(2000000)
```

We already showed how our global invariants initially held for the starting values of the global variables (`balance = 0` and `elite = false`). When we consider the first function call, `deposit(500000)`, we can also see that the precondition holds (we are depositing a non-negative amount). The `deposit` postcondition tells us that the new value of `balance` is 500000 more than it was before the function call, so we know balance is now 500000. We can also assume that all global invariants hold after the `deposit` call, so we can infer that `elite` is still false (since the balance is not more than the threshold). Thus the next assert statement:

```text
assert(balance == 500000 & elite == false)
```

will hold in Logika's symexe mode.

The very next statement in the calling code is another call to `deposit`. Since we could assume the global invariants held immediately after the last call to deposit, we can infer that they still hold before the next `deposit` call. We also see that the function's precondition is satisfied, as we are depositing another nonnegative value. Just as before, we can use the `deposit` postcondition to see that `balance` will be 1000000 after the next function call (the postcondition tells us that `balance` is 500000 more than it was just before the function call). We also know that the global invariants hold, so we are sure `elite` has been updated to true. Thus our next assert holds as well:

```text
assert(balance == 1000000 & elite == true)
```

Our final function call, `withdraw(2000000)`, will not be allowed. We are trying to withdraw $2,000,000, but our account balance at this point is $1,000,000. We will get an error saying that the `withdraw` precondition has not been satisfied, as that function requires that our withdrawal amount be less than or equal to the account balance.