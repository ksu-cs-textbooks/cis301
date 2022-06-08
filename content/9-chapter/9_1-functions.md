---
title: "Functions"
pre: "9.1. "
weight: 100
date: 2018-08-24T10:53:26-05:00
---

A *function* in Logika is analogous to a method in Java or C# -- it is a named body of commands that does significant work. It may take one or more parameters and/or return a value. Recall the syntax for a function in Logika:

```text
def functionName(paramList): returnType = {

}
```

We will use the keyword `Unit` (like `void` in other languages) for a function that does not return a value. If a function has a non-`Unit` return type, then all paths through the function must end in a return statement:

```text
return expression
```

Where `expression` is a variable, literal, or expression matching `returnType`.

## Function contracts

In order to prove the correctness of a function, it must include a *function contract* just inside the function header. This function contract specifies what we need to know to use the function correctly. While we will use these specifications to help us prove correctness, they are good to include in ANY library function that will be used by someone else (even more informally in other languages). *The person/program who calls the function is not supposed to read the function's code to know how to use the function*. A function contract shows them what parameters to pass (including what range of values are acceptable), what the function will return in terms of the parameter, and whether/how the function will modify things like sequences (arrays/lists in other languages) or global variables.

Here is the syntax for a function contract in Logika:

```text
l"""{
    requires (preconditions)
    modifies (variable list)
    ensures (postconditions)
}"""
```

Here is a summary of the different function contract clauses:

- `requires`: lists the *preconditions* for the function. We can also use the keyword `pre` instead of `requires`. If there are no preconditions, we can skip this clause. If we have multiple preconditions, we can list them on separate lines (where subsequent lines are tabbed over under `requires`).
- `modifies`: lists the name of any sequence parameters and/or global variables that are modified by the function. We can skip this clause until chapter 10, when we will see sequences and global variables.
- `ensures`: lists the *postconditions* for the function. We can also use the keyword `post` instead of `ensures`. If we have multiple postconditions, we can list them on separate lines (where subsequent lines are tabbed over under `ensures`).

### Preconditions

The *preconditions* for a function are requirements the function has in order to operate correctly. Generally, preconditions constrain the values of the parameters and/or global variables. For example, this function returns the integer division between two parameters. The function can only operate correctly when the denominator (the second parameter, `b`) is non-zero:

```text
def div(a: Z, b: Z) : Z =  {
    l"""{
        requires b != 0
        ...
    }"""

    val ans: Z = a/b
    return ans
}
```

Logika will throw an error if any preconditions are not proven before calling a function. Because we are required to prove the preconditions before any function call, the function itself can list the preconditions as premises in a logic block just after the function contract:

```text
def example(a: Z, b: Z) : Z =  {
    l"""{
        requires precondition1
            precondition2
            ...
        ...
    }"""

    //we can list the preconditions as premises
    l"""{
        1. precondition1            premise
        2. precondition2            premise
        ...
    }"""

    ...
}
```

### Postconditions

The *postconditions* of a function state what the function has accomplished when it terminates. In particular, postconditions should include:

- A formalization of what the function promises to return in terms of the parameters/global variables. We can use the keyword `result` to refer to the object returned by the function (we will only use this keyword in the function contract).

- A description of how any global variables and/or sequence parameters will be modified by the function (we will not use global variables or sequences until chapter 10).

For example, the `div` function above should promise to return the integer division of its two parameters, like this:

```text
def div(a: Z, b: Z) : Z =  {
    l"""{
        requires b != 0
        ensures result == a/b
    }"""

    val ans: Z = a/b
    return ans
}
```

In order to prove a postcondition involving the return value, we must have logic blocks just before returning that demonstrate each postcondition claim, using the variable name being returned instead of the `result` keyword. In the example above, since we are returning a variable named `ans`, then we must prove the claim `ans == a/b` in order to satisfy the postcondition. We can complete the verification of the `div` function as follows:

```text
def div(a: Z, b: Z) : Z =  {
    l"""{
        requires b != 0
        ensures result == a/b
    }"""

    val ans: Z = a/b

    l"""{
        1. b != 0               premise     //precondition (needed for division)
        2. ans == a/b           premise     //satisifies the postcondition
                                            //(from the "ans = a/b" assignment)
    }"""

    return ans
}
```

Logika will throw an error if postconditions are not proven before leaving a function. Because we are required to prove the postconditions before the function ends, any calling code can list those postconditions as premises after calling the function. We will see this in more detail in the next section.

## Work of the calling code

We saw above that when writing code that calls a function, we must PROVE the preconditions before the function call (since the function requires that we meet those preconditions in order to work correctly). After the function terminates, the calling code can list the function's postconditions as PREMISES (since the function ensured that certain things would happen).

Below, we will see the syntax for the verification of code that calls a function. We will refer to our finished `div` function:

```text
def div(a: Z, b: Z) : Z =  {
    l"""{
        requires b != 0
        ensures result == a/b
    }"""

    val ans: Z = a/b

    l"""{
        1. b != 0               premise     //precondition (needed for division)
        2. ans == a/b           premise     //satisifies the postcondition
                                            //(from the "ans = a/b" assignment)
    }"""

    return ans
}
```

The "calling code" in Logika goes outside of any function definition. Typically, I place the calling code at the bottom of the Logika file, after all functions. Recall that this is the code executed first by Logika, just like in Python programs.

### Proving preconditions

Suppose we wish to call the `div` function to divide two numbers:

```text
val x: Z = 10
val y: Z = 2

val num: Z = div(x, y)
```

If we included that calling code in a Logika file with our `div` function, we would see an error that we had not yet proved the precondition. To prove each precondition, we must have a logic block just before the function call that demonstrate each precondition claim, using value being passed instead of the parameter name. Since we are passing the value `y` as our second parameter, and since the `div` function requires that `b != 0` (where `b` is the second parameter), then we must demonstrate that `y != 0`:

```text
val x: Z = 10
val y: Z = 2

l"""{
    1. x == 10              premise     //from the "x = 10" assignment
    2. y == 2               premise     //from the "y = 2" assignment
    3. y != 0               algebra 2   //satisifies the precondition for div
}"""

val num: Z = div(x, y)
```

Note that Logika is picky about proving the precondition using EXACTLY the value being passed for the corresponding parameter. For example, suppose instead that we wanted to divide `x-1` and `y+1`:

```text
val x: Z = 10
val y: Z = 2

l"""{
    1. x == 10              premise     //from the "x = 10" assignment
    2. y == 2               premise     //from the "y = 2" assignment
    3. y != 0               algebra 2   //NO! precondition is not satisifed!
}"""

val num: Z = div(x-1, y+1)
```

If we made no changes to our logic block, Logika would complain that we had not satisfied the precondition. And indeed we haven't -- while we've shown that `y` isn't 0, we haven't shown that our second parameter `y+1` isn't 0. Here is the correction: 

```text
val x: Z = 10
val y: Z = 2

l"""{
    1. x == 10              premise     //from the "x = 10" assignment
    2. y == 2               premise     //from the "y = 2" assignment
    3. y+1 != 0             algebra 2   //Yes! Satisfies the precondition for our second parameter (y+1)
}"""

val num: Z = div(x-1, y+1)
```

### Using postconditions 

Recall that since the function is ensuring that it will do/return specific things (its postconditions), then the calling code can list those postconditions as premises after the function call. If a postcondition uses the keyword `result`, then the calling code can list exactly that postcondition using whatever variable it used to store the return value in place of `result` and using whatever values were passed in place of the parameter names. In the `div` example above where we divde `x-1` by `y+1`, the calling code stores the value returned by `div` in a variable called `num`. Since the `div` postcondition is `result == a/b`, then we can claim the premise `num == (x-1)/(y+1)`:

```text
val x: Z = 10
val y: Z = 2

l"""{
    1. x == 10              premise     //from the "x = 10" assignment
    2. y == 2               premise     //from the "y = 2" assignment
    3. y+1 != 0             algebra 2   //Satisfies the precondition for our second parameter (y+1)
}"""

val num: Z = div(x-1, y+1)

l"""{
    1. num == (x-1)/(y+1)   premise     //postcondition of div
}"""
```

We know from looking at this example that `(x-1)/(y+1)` is really `9/3`, which is 3. We would like to be able to assert that `num`, the value returned from `div`, equals 3. We can do this by adding a few more steps to our final logic block, plugging in the values for `x` and `y` and doing some algebra:

```text
val x: Z = 10
val y: Z = 2

l"""{
    1. x == 10              premise     //from the "x = 10" assignment
    2. y == 2               premise     //from the "y = 2" assignment
    3. y+1 != 0             algebra 2   //Satisfies the precondition for our second parameter (y+1)
}"""

val num: Z = div(x-1, y+1)

l"""{
    1. num == (x-1)/(y+1)   premise     //postcondition of div
    2. x == 10              premise     //x is unchanged from the "x = 10" assignment
    3. y == 2               premise     //y is unchanged from the "y = 2" assignment
    4. num == 9/3           algebra 1 2 3
    5. num == 3             algebra 4   //needed for assert
}"""

assert(num == 3)
```

Using a function contract and our deduction rules, we have PROVED that the `div` function will return 3 in our example (without needing to test the code at all).

## Examples

In this section, we will see two completed examples of Logika programs with a function and calling code.

### Example 1

In this example, we write a `plusOne` function that takes a non-negative parameter and returns one more than that parameter:

```text
import org.sireum.logika._

def plusOne(n: Z): Z = {
    l"""{
        requires n >= 0         //precondition: parameter should be non-negative
        ensures result == n+1   //postcondition 1: we promise returned value is one more than parameter
            result > 0          //postcondition 2: we promise returned value is greater than 0
    }"""


    val answer: Z = n+1
    l"""{
        1. n >= 0               premise     //from the precondition
        2. answer == n+1        premise     //from the "answer = n+1" assignment
                                            //proves the first postcondition

        3. answer > 0           algebra 1 2 //proves the second postcondition
    }"""

    return answer
}

////////// Test code ///////////////

var x: Z = 5

l"""{
    1. x == 5                   premise     //from the "x=5" assignment
    2. x >= 0                   algebra 1   //proves the plusOne precondition
}"""

var added: Z = plusOne(x)

l"""{
    //I can list the postcondition (what is returned) as a premise
    1. x == 5                   premise     //x is unchanged 
    2. added == x+1             premise     //plusOne postcondition 1
    3. added > 0                premise     //plusOne postcondition 2
    4. added == 6               algebra 1 2
    5. added == 6 
}"""

assert(added == 6 ∧ added > 0)
```

Note that when we have more than one postcondition, we must prove all postconditions inside the function, and we can list all postconditions as premises in the calling code after the function call.

### Example 2

In this example, we write a `findMax` function that returns the biggest between two integer parameters. This is very similar to an example from section 8.7, which used an if/else statement to find the max. In that example, our assert that we had indeed found the max was: `assert((max >= x & max >= y) & (max == x | max == y))`. We will see that our postconditions for `findMax` come directly from the different claims in that assert. In our calling code, we call `findMax` with `num1` (which has the value 3) and `num2` (which has the value 2). We are able to prove that `findMax` returns 2:

```text
import org.sireum.logika._

//find the max between x and y
def findMax(x: Z, y: Z): Z = {
    l"""{
        //no precondition needed
        ensures
            result >= x                 //postcondition 1
            result >= y                 //postcondition 2
            result == x v result == y   //postcondition 3
    }"""

    var max: Z = 0

    l"""{
        1. max == 0             premise
    }"""

    if (x > y) {
        l"""{
            1. max == 0         premise
            2. x > y            premise     //IF condition is true
        }"""

        max = x

        l"""{
            1. max == x         premise
            2. max >= x         algebra 1
            3. x > y            premise
            4. max >= y         algebra 1 3
        }"""

    } else {
        l"""{
            1. max == 0         premise
            2. ¬(x > y)         premise     //IF condition is not true
            3. x <= y           algebra 2
        }"""

        max = y
        l"""{
            1. max == y         premise
            2. x <= y           premise
            3. max >= x         algebra 1 2
            4. max >= y         algebra 1
        }"""
    }

    //prove the postconditions
    l"""{
        //true in both the if and the else
        1. max >= x                 premise     //proves postcondition 1 
        2. max >= y                 premise     //proves postcondition 2

        //first was true in if, second true in else
        3. max == x v max == y      premise     //proves postcondition 3
    }"""

    return max
}

////////////// Test code /////////////////

val num1: Z = 3
val num2: Z = 2

//findMax has no preconditions, so nothing to prove here

val biggest: Z = findMax(num1, num2)

l"""{
    1. biggest >= num1                      premise     //findMax postcondition 1
    2. biggest >= num2                      premise     //findMax postcondition 2
    3. biggest == num1 v biggest == num2    premise     //findMax postcondition 3

    //pull in the initial values
    4. num1 == 3                            premise
    5. num2 == 2                            premise

    6. biggest >= 3                         algebra 1 4
    7. biggest >= 2                         algebra 2 5
    8. biggest == 3 v biggest == num2       subst1 4 3
    9. biggest == 3 v biggest == 2          subst1 5 8

    //OR-elimination
    10. {
        11. biggest == 3                    assume
    }
    12. {
        13. biggest == 2                    assume
        14. ¬(biggest >= 3)                 algebra 6 13
        15. ⊥                               ¬e 6 14
        16. biggest == 3                    ⊥e 15
    }
    17. biggest == 3                        ve 9 10 12  //needed for assert
}"""

assert(biggest == 3)
```