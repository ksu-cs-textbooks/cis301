---
title: "Functions"
pre: "9.1. "
weight: 100
date: 2018-08-24T10:53:26-05:00
---

A *function* in Scala is analogous to a method in Java or C# -- it is a named body of commands that does significant work. It may take one or more parameters and/or return a value. Recall the syntax for a function in Scala:

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
Contract(
    Requires (  preconditions   ),
    Ensures (   postconditions  )
)
```

Here is a summary of the different function contract clauses:

- `Requires`: lists the *preconditions* for the function in a comma-separated list. If there are no preconditions, we can skip this clause.
- `Ensures`: lists the *postconditions* for the function in a comma-separated list. 

### Preconditions

The *preconditions* for a function are requirements the function has in order to operate correctly. Generally, preconditions constrain the values of the parameters and/or global variables. For example, this function returns the integer division between two parameters. The function can only operate correctly when the denominator (the second parameter, `b`) is non-zero:

```text
def div(a: Z, b: Z) : Z =  {
    Contract(
        Requires (  b != 0  ),
        ...
    )

    val ans: Z = a/b
    return ans
}
```

Logika will display an error if any preconditions are not proven before calling a function. Because we are required to prove the preconditions before any function call, the function itself can list the preconditions as premises in a proof block just after the function contract:

```text
def example(a: Z, b: Z) : Z =  {
    Contract(
        Requires (  Precondition1, Precondition2, ...),
        ...
    )

    //we can list the preconditions as premises
    Deduce(
        1 ( precondition1   )   by Premise,
        2 ( precondition2   )   by Premise
        ...
    )

    ...
}
```

### Postconditions

The *postconditions* of a function state what the function has accomplished when it terminates. In particular, postconditions should include:

- A formalization of what the function promises to return in terms of the parameters/global variables. We use the keyword `Res[returnType]` to refer to the object returned by the function (we will only use this keyword in the function contract). For example, in a function that returns an integer (`Z`), we can use the keyword `Res[Z]` in a postcondition to refer to the return value.

- A description of how any global variables and/or sequence parameters will be modified by the function (we will not use global variables or sequences until chapter 10).

For example, the `div` function above should promise to return the integer division of its two parameters, like this:

```text
def div(a: Z, b: Z) : Z =  {
    Contract(
        Requires(   b != 0  ),
        Ensures(    Rez[Z] == a/b   )
    )

    val ans: Z = a/b
    return ans
}
```

In order to prove a postcondition involving the return value, we must have proof blocks just before returning that demonstrate each postcondition claim, using the variable name being returned instead of the `Res[returnType]` keyword. In the example above, since we are returning a variable named `ans`, then we must prove the claim `ans == a/b` in order to satisfy the postcondition. We can complete the verification of the `div` function as follows:

```text
def div(a: Z, b: Z) : Z =  {
    Contract(
        Requires(   b != 0  ),
        Ensures(    Res[Z] == a/b   )
    )

    val ans: Z = a/b

    Deduce(
        1 ( b != 0      )   by Premise, //precondition (needed for division)
        2 ( ans == a/b  )   by Premise  //satisifes the postcondition
                                        //(from the "ans = a/b" assignment)
    )

    return ans
}
```

Logika will display an error if postconditions are not proven before leaving a function. Because we are required to prove the postconditions before the function ends, any calling code can list those postconditions as premises after calling the function. We will see this in more detail in the next section.

## Work of the calling code

We saw above that when writing code that calls a function, we must PROVE the preconditions before the function call (since the function requires that we meet those preconditions in order to work correctly). After the function terminates, the calling code can list the function's postconditions as PREMISES (since the function ensured that certain things would happen).


The "calling code" in Scala goes outside of any function definition. Typically, I place the calling code at the bottom of the Scala file, after all functions. This is the code executed first by Scala, just like in Python programs.

### Proving preconditions

Suppose we wish to call the `div` function to divide two numbers:

```text
val x: Z = 10
val y: Z = 2

val num: Z = div(x, y)
```

If we included that calling code in a file with our `div` function, we would see an error that we had not yet proved the precondition. To prove each precondition, we must have a proof block just before the function call that demonstrate each precondition claim, using value being passed instead of the parameter name. Since we are passing the value `y` as our second parameter, and since the `div` function requires that `b != 0` (where `b` is the second parameter), then we must demonstrate that `y != 0`:

```text
val x: Z = 10
val y: Z = 2

Deduce(
    1 ( x == 10     )   by Premise,     //from the "x = 10" assignment
    2 ( y == 2      )   by Premise,     //from the "y = 2" assignment
    3 ( y != 0      )   by Algebra*(2)  //satisfies the precondition for div
)

val num: Z = div(x, y)
```

Note that Logika is picky about proving the precondition using EXACTLY the value being passed for the corresponding parameter. For example, suppose instead that we wanted to divide `x-1` and `y+1`:

```text
val x: Z = 10
val y: Z = 2

Deduce(
    1 ( x == 10     )   by Premise,     //from the "x = 10" assignment
    2 ( y == 2      )   by Premise,     //from the "y = 2" assignment
    3 ( y != 0      )   by Algebra*(2)  //NO! precondition is not satisfied!
)

val num: Z = div(x-1, y+1)
```

If we made no changes to our logic block, Logika would complain that we had not satisfied the precondition. And indeed we haven't -- while we've shown that `y` isn't 0, we haven't shown that our second parameter `y+1` isn't 0. Here is the correction: 

```text
val x: Z = 10
val y: Z = 2

Deduce(
    1 ( x == 10     )   by Premise,     //from the "x = 10" assignment
    2 ( y == 2      )   by Premise,     //from the "y = 2" assignment
    3 ( y+1 != 0    )   by Algebra*(2)  //Yes! Satisfies the precondition for our second parameter (y+1)
)

val num: Z = div(x-1, y+1)
```

### Using postconditions 

Recall that since the function is ensuring that it will do/return specific things (its postconditions), then the calling code can list those postconditions as premises after the function call. If a postcondition uses the keyword `Res[returnType]`, then the calling code can list exactly that postcondition using whatever variable it used to store the return value in place of `Res[returnType]` and using whatever values were passed in place of the parameter names. In the `div` example above where we divide `x-1` by `y+1`, the calling code stores the value returned by `div` in a variable called `num`. Since the `div` postcondition is `result == a/b`, then we can claim the premise `num == (x-1)/(y+1)`:

```text
val x: Z = 10
val y: Z = 2

Deduce(
    1 ( x == 10     )   by Premise,     //from the "x = 10" assignment
    2 ( y == 2      )   by Premise,     //from the "y = 2" assignment
    3 ( y+1 != 0    )   by Algebra*(2)  //Yes! Satisfies the precondition for our second parameter (y+1)
)

val num: Z = div(x-1, y+1)

Deduce(
    1 ( num == (x-1)/(y+1)  )   by Premise  //postcondition of div
)
```

We know from looking at this example that `(x-1)/(y+1)` is really `9/3`, which is 3. We would like to be able to assert that `num`, the value returned from `div`, equals 3. We can do this by adding a few more steps to our final proof block, plugging in the values for `x` and `y` and doing some algebra:

```text
val x: Z = 10
val y: Z = 2

Deduce(
    1 ( x == 10     )   by Premise,     //from the "x = 10" assignment
    2 ( y == 2      )   by Premise,     //from the "y = 2" assignment
    3 ( y+1 != 0    )   by Algebra*(2)  //Yes! Satisfies the precondition for our second parameter (y+1)
)

val num: Z = div(x-1, y+1)

Deduce(
    1 ( num == (x-1)/(y+1)  )   by Premise,     //postcondition of div
    2 ( x == 10             )   by Premise,     //previous variable assignment
    3 ( y == 2              )   by Premise,     //previous variable assignment
    4 ( num == 9/3          )   by Algebra*(1,2,3),
    5 ( num == 3            )   by Algebra*(4)  //needed for assert 
)

assert(num == 3)
```

Using a function contract and our deduction rules, we have PROVED that the `div` function will return 3 in our example (without needing to test the code at all).

## Examples

In this section, we will see two completed examples of Logika programs with a function and calling code.

### Example 1

In this example, we write a `plusOne` function that takes a non-negative parameter and returns one more than that parameter:

```text
// #Sireum #Logika
//@Logika: --manual --background save
import org.sireum._
import org.sireum.justification._
import org.sireum.justification.natded.prop._

def plusOne(n: Z): Z = {
    Contract(
        Requires(   n >= 0  ),
        Ensures(
            Res[Z] == n + 1,
            Res[Z] > 0
        )
    )

    val answer: Z = n + 1
  
    Deduce(
        1  (    n >= 0          )   by Premise,
        2  (    answer == n + 1 )   by Premise,
        3  (    answer > 0      )   by Algebra*(1, 2)
    )

    return answer
}

////////// Test code ///////////////

var x: Z = 5
Deduce(
    1  (    x == 5  )   by Premise,
    2  (    x >= 0  )   by Algebra*(1)
)

var added: Z = plusOne(x)

Deduce(
    1  (    x == 5                  ) by Premise,
    2  (    added == x + 1          ) by Premise,
    3  (    added > 0               ) by Premise,
    4  (    added == 6              ) by Algebra*(1, 2),
    5  (    added == ∧ & added > 0  ) by AndI(4, 3)
)
assert(added == 6 ∧ added > 0)

var x: Z = 5

Deduce(
    1 (     x == 5      )   by Premise,     //from the "x=5" assignment
    2 (     x >= 0      )   by Algebra*(1)  //proves the plusOne precondition
)

var added: Z = plusOne(x)

Deduce(
    //I can list the postcondition (what is returned) as a premise
    1 (     x == 5                  )   by Premise, //x is unchanged 
    2 (     added == x+1            )   by Premise, //plusOne postcondition 1
    3 (     added > 0               )   by Premise, //plusOne postcondition 2
    4 (     added == 6              )   by Algebra*(1,2),
    5 (     added == 6 ∧ added > 0  )   by AndI(4,3)
)

assert(added == 6 ∧ added > 0)
```

Note that when we have more than one postcondition, we must prove all postconditions inside the function, and we can list all postconditions as premises in the calling code after the function call.

### Example 2

In this example, we write a `findMax` function that returns the biggest between two integer parameters. This is very similar to an example from section 8.7, which used an if/else statement to find the max. In that example, our assert that we had indeed found the max was: `assert((max >= x & max >= y) & (max == x | max == y))`. We will see that our postconditions for `findMax` come directly from the different claims in that assert. In our calling code, we call `findMax` with `num1` (which has the value 3) and `num2` (which has the value 2). We are able to prove that `findMax` returns 2:

```text
// #Sireum #Logika
//@Logika: --manual --background save
import org.sireum._
import org.sireum.justification._
import org.sireum.justification.natded.prop._

//find the max between x and y
def findMax(x: Z, y: Z): Z = {
    Contract(
        //no precondition needed
        Ensures(
            Res[Z] >= x,                //postcondition 1
            Res[Z] >= y,                //postcondition 2
            Res[Z] == x v Res[Z] == y   //postcondition 3
        )
    )

    var max: Z = 0

    if (x > y) {
        max = x

        Deduce(
            1 (     max == x    )   by Premise,
            2 (     max >= x    )   by Algebra*(1),     //build to postcondition 1
            3 (     x > y       )   by Premise,         //IF condition is true
            4 (     max >= y    )   by Algebra*(1,3)    //build to postcondition 2
        )

    } else {
        max = y

        Deduce(
            1 (     max == y    )   by Premise,
            2 (     ¬(x > y)    )   by Premise,         //IF condition is false
            3 (     x <= y      )   by Algebra*(2),
            4 (     max >= x    )   by Algebra*(1, 2),  //build to postcondition 1
            5 (     max >= y    )   by Algebra*(1)      //build to postcondition 2
        )
    }

    //prove the postconditions
    Deduce(
        //true in both the if and the else
        1 (     max >= x            )   by Premise,     //proves postcondition 1 
        2 (     max >= y            )   by Premise,     //proves postcondition 2

        //first was true in if, second true in else
        3 (     max == x v max == y )   by Premise     //proves postcondition 3
    )

    return max
}

////////////// Test code /////////////////

val num1: Z = 3
val num2: Z = 2

//findMax has no preconditions, so nothing to prove here

val biggest: Z = findMax(num1, num2)

Deduce(
    1 (     biggest >= num1                     )   by Premise,     //findMax postcondition 1
    2 (     biggest >= num2                     )   by Premise,     //findMax postcondition 2
    3 (     biggest == num1 v biggest == num2   )   by Premise,     //findMax postcondition 3

    //pull in the initial values
    4 (     num1 == 3                           )   by Premise,
    5 (     num2 == 2                           )   by Premise,

    6 (     biggest >= 3                        )   by Algebra*(1, 4),
    7 (     biggest >= 2                        )   by Algebra*(2, 5),
    8 (     biggest == 3 v biggest == num2      )   by Subst_<(4, 3),
    9 (     biggest == 3 v biggest == 2         )   by Subst_<(5, 8),

    //OR-elimination
    10 SubProof(
        11  Assume( biggest == 3 ) 
    ),
    12 SubProof(
        13  Assume( biggest == 2 ) 
        14 (    ¬(biggest >= 3)                 )   by Algebra*(13),
        15 (    F                               )   by NegE(6, 14),
        16 (    biggest == 3                    )   by BottomE(15)
    ),
    17 (        biggest == 3                    )   by OrE(9,10,12) //needed for assert
)

assert(biggest == 3)
```