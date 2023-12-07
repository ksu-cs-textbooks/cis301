---
title: "Logika Facts"
pre: "9.4. "
weight: 103
date: 2018-08-24T10:53:26-05:00
---

We saw at the end of section 9.3 that we sometimes need a more expressive way of specifying a loop invariant (or, similarly, for postconditions). In our last example, we wanted to describe the factorial operation. We know that {{< math >}}$n! = n * (n-1) * (n-2) * ... * 2 * 1${{< /math >}}, but we don't have a way to describe the "..." portion using our current tools.

In this section, we introduce *Logika facts*, which will let us create our own recursive proof functions that we can use in invariants and postconditions. We will usually want to use a Logika fact anytime our invariant or postcondition needs to express something that has a "..." to demonstrate a pattern.

## Logika fact syntax

Logika allows these proof functions to be written in multiple ways, but we will start with the most straightforward of these options:

```text
l"""{
    fact
        def proofFunctionName(paramList) : returnType
            factName1. //describe when proofFunctionName has its first possible value
            ...
            factNameN. //describe when proofFunctionName has its last possible value
}"""
```

In the proof function definition, *proofFunctionName* is the name we give our proof function, *paramList* is the list of parameter names and types needed for this proof function (which are formatted exactly like a parameter list in a regular Logika function), and *returnType* is the return type of the proof function (usually either `Z` for integer or `B` for boolean).

Below the proof function definition, we include a line for each possible way to calculate its value. Usually, at least one of the lines includes a recursive defintion -- relating the value of something like `proofFunctionName(n)` to the proof function's definition for a smaller value, like `proofFunctionName(n-1)`. The label, such as `factNameN`, names the proof rule. We will be able to pull in a particular line of the definition into a logic block by using the justification `fact factNameN`.

Logika facts are defined at the top of the Logika file, below the `import` but before any of the code.

## Example: Logika fact to define factorial

It is much easier to see how Logika facts work by using an example. Suppose we want to define the factorial operation. The first step is to come up with a *recursive definition*, which has us defining the operation the same way we would in a recursive function -- with one or more *base cases* where we can define the operation for the simplest case, and one or more *recursive cases* that express a general instance of the problem in terms of a smaller instance.

For factorial, the simplest version is {{< math >}}$1!${{< /math >}}, which is just 1. In the general case, we have that:

```math
$$
n! = n * (n-1) * (n-2) * ... * 2 * 1 = n * (n-1)!
$$
```
<br>

So we can write the following recursive definition:

- Base case: {{< math >}}$1! = 1${{< /math >}}
- Recursive case: for values of {{< math >}}$n${{< /math >}} greater than 1, {{< math >}}$n! = n * (n-1)!${{< /math >}}

And we can then translate the recursive definition to a Logika fact:

```text
l"""{
    fact
        def factDef(n: Z): Z
            fOne. factDef(1) == 1
            fBig.  ∀x: Z  x > 1 → factDef(x) == x * factDef(x - 1)
}"""
```

Let's consider each portion of this proof function. Here, `factDef` is the name given to the proof function. It takes one parameter, `n`, which is an integer, and it returns an integer. We have two possible ways of calculating the value for the proof function. First, we define `fOne`:

```text
fOne. factDef(1) == 1
```

`fOne` defines `factDef(1)` as 1; i.e., `factDef(n)` is 1 if {{< math >}}$n == 1${{< /math >}}. This is the same as our base case in our recursive definition for factorial -- {{< math >}}$1! = 1${{< /math >}}.

Next, consider the definition for `fBig`:

```text
fBig.  ∀x: Z  x > 1 → factDef(x) == x * factDef(x - 1)
```

`fBig` states that for all integers `x` that are bigger than 1, we define `factDef(x) == x * factDef(x - 1)`. This is the same as our recursive case in our recursive definition for factorial -- for values of {{< math >}}$n${{< /math >}} greater than 1, {{< math >}}$n! = n * (n-1)!${{< /math >}}.

## Evaluating a Logika fact

Suppose we used our `factDef` proof function to calculate `factDef(3)`. We would have:

```text
factDef(3) == 3 * factDef(2)        //we use fBig, since 3 > 1
factDef(2) == 2 * factDef(1)        //we use fBig, since 2 > 1
factDef(1) == 1                     //we use fOne       
```

Once we work down to:

```text
factDef(1) == 1
```
 
We can plug `1` in for `factDef(1)` in `factDef(2) == 2 * factDef(1)`, which gives us:

```text
factDef(2) == 2
```

Similarly, we can plug `2` in for `factDef(2)` in `factDef(3) == 3 * factDef(2)`, and see that:

```text
factDef(3) == 6
```

## Using Logika facts as justifications

If we had our Logika fact, `factDef`, then we could pull its two definitions into a logic block like this:

```text
l"""{
    1. factDef(1) == 1                                  fact fOne
    2. ∀x: Z  x > 1 → factDef(x) == x * factDef(x - 1)  fact fBig
}"""
```

Note that we must pull in the definitions EXACTLY as they are written in the proof function. The justification is always `fact` followed by the name of the corresponding definition.

## Using Logika facts in postconditions and invariants

Consider the following full Logika program that includes a function to find and return a factorial, as well as test code that calls our factorial function:

```text
import org.sireum.logika._

// n! = n * (n-1) * (n-2) * .. * 1
// 1! = 1
def factorial(n: Z): Z = {
    var i: Z = 1        //how many multiplications we have done
    var product: Z = 1  //our current calculation
    while (i != n) {
        i = i + 1
        product = product * i
    }

    return product
}

//////// Test code ///////////

var num: Z = 2

var answer: Z = factorial(num)

assert(answer == 2)
```

We want to add a function contract, loop invariant block, and supporting logic blocks to demonstrate that `factorial` is returning `n!` and to prove the assert in our text code. 

### Writing a function contract using a Logika fact

We want our `factorial` function contract to say that it is returning `n!`, and that it is only defined for values of `n` that are greater than or equal to 0. We recall that our Logika fact, `factDef`, defines the factorial operation:

```text
l"""{
    fact
        def factDef(n: Z): Z
            fOne. factDef(1) == 1
            fBig.  ∀x: Z  x > 1 → factDef(x) == x * factDef(x - 1)
}"""
```

And we will use `factDef` to define what we mean by "factorial" in our `factorial` function contract:

```text
def factorial(n: Z): Z = {
    l"""{
        requires n >= 1                 //factorial(n) is only defined when n >= 1
        ensures result == factDef(n)    //we promise to return factDef(n),
                                        //where factDef(n) defines n!
    }"""

    //code for factorial function
}
```

### Writing a loop invariant block using a Logika fact

We can similarly use `factDef` in our loop invariant block. We noted at the end of section 9.3 that the invariant in our loop should be: *prod equals i factorial*. Now we have a way to express what we mean by "factorial", so our invariant will be: `prod == factDef(i)`. Since the `factDef` proof function is only defined for parameters greater than or equal to 1, we need to add a second piece to the invariant to guarantee that `i` will always be greater than or equal to 1. We now have the following loop invariant block:

```text
while (i != n) {
    l"""{
        invariant product == factDef(i)
            i >= 1
        modifies i, product
    }"""

    //loop body
}
```

### Finishing the verification

All that remains is to:

- Prove our loop invariant holds before the loop begins
- When we assume the loop invariant holds at the beginning of an iteration, prove that it still holds at the end of the iteration
- Use the loop invariant together with the negation of the loop condition to prove the `factorial` postcondition
- Prove the precondition holds in the calling code just before calling `factorial`
- Use the postcondition after calling `factorial` to prove the final assert

Here is the completed verification:

```text
import org.sireum.logika._

l"""{
    fact
        def factDef(n: Z): Z
            fOne. factDef(1) == 1
            fBig.  ∀x: Z  x > 1 → factDef(x) == factDef(x - 1) * x
}"""

def factorial(n: Z): Z = {
    l"""{
        requires n >= 1
        ensures result == factDef(n)
    }"""

    var i: Z = 1 //how many multiplications we have done
    var product: Z = 1  //my current calculation

    //Prove invariant before loop begins
    l"""{
        1. i == 1                       premise
        2. product == 1                 premise

        //pull in first Logika fact rule
        3. factDef(1) == 1              fact fOne     

        //proves first loop invariant holds  
        4. product == factDef(i)        algebra 1 2 3   

        //proves second loop invariant holds
        5. i >= 1                       algebra 1       
    }"""

    while (i != n) {
        l"""{
            invariant product == factDef(i)
                i >= 1
            modifies i, product
        }"""

        i = i + 1

        l"""{
            //from "i = i + 1"
            1. i == i_old + 1               premise     

            //loop invariant held before changing i
            2. product == factDef(i_old)    premise     

            //rewrite invariant with no "_old"
            3. product == factDef(i-1)      algebra 1 2 

            //second loop invariant held before changing i
            4. i_old >= 1                   premise     

            //needed for the Logika fact
            5. i > 1                        algebra 1 4 
        }"""

        product = product * i

        //Prove invariant still holds at end of iteration
        l"""{
            //from "product = product * i"
            1. product == product_old*i                         premise 

            //from previous logic block
            2. product_old == factDef(i-1)                      premise 

            //pull in Logika fact
            3. ∀x: Z  x > 1 → factDef(x) == factDef(x - 1) * x  fact fBig

            //plug in "i" for "x"
            4. i > 1 → factDef(i) == factDef(i - 1) * i         Ae 3 i

            //from previous logic block
            5. i > 1                                            premise   

            //i > 1, so get right side of →
            6. factDef(i) == factDef(i - 1) * i                 →e 4 5     
            7. product == factDef(i-1)*i                        algebra 1 2

            //proves first invariant still holds
            8. product == factDef(i)                            algebra 6 7 

            //proves first invariant still holds
            9. i >= 1                                           algebra 5
        }"""
    }

    //Prove postcondition
    l"""{
        1. product == factDef(i)        premise //loop invariant
        2. !(i != n)                    premise //loop condition false
        3. i == n                       algebra 2
        4. product == factDef(n)        algebra 1 3
    }"""

    return product
}

//////// Test code ///////////

var num: Z = 2

//Prove precondition
l"""{
    1. num == 2             premise
    2. num >= 1             algebra 1   //proves factorial precondition
}"""

var answer: Z = factorial(num)

l"""{
    1. answer == factDef(num)           premise     //factorial postcondition
    2. num == 2                         premise
    3. answer == factDef(2)             algebra 1 2

    //pull in Logika fact
    4. ∀x: Z  x > 1 → factDef(x) == factDef(x - 1) * x  fact fBig

    //plug in "2" for "x"
    5. 2 > 1 → factDef(2) == factDef(2 - 1) * 2         Ae 4 2 
    6. 2 > 1                            algebra

    //2 > 1, so use →
    7. factDef(2) == factDef(2 - 1) * 2  →e 5 6  

    //pull in Logika fact
    8. factDef(1) == 1                  fact fOne       
    9. factDef(2) == factDef(1) * 2     algebra 7
    10. factDef(2) == 2                 algebra 8 9

    //proves claim in assert
    11. answer == 2                     algebra 1 2 10
}"""

assert(answer == 2)
```

## Logika fact for multiplication

Suppose we wanted to create a Logika fact that recursively defined multiplication. We first recursively define how we would multiply {{< math >}}$x * y${{< /math >}}. We know that our base case will be when {{< math >}}$y == 0${{< /math >}}, because anything times 0 is 0. We also saw that multiplication can be defined as repeated addition, so that {{< math >}}$x * y == x + x + ... x${{< /math >}} for a total of {{< math >}}$y${{< /math >}} additions. We also see that {{< math >}}$x * y == x + x * (y-1)${{< /math >}}, since we can pull out one of the additions and then have {{< math >}}$y-1${{< /math >}} additions left to do. 

Here is our recursive definition of the problem:

- Base case: for all numbers x, x * 0 is 0
- Recursive case: for all numbers x and all positive numbers y, x * y = x + x * (y-1)

We can translate this directly to a Logika fact:

```text
l"""{
    fact
        //defines m * n = m + m + ... + m (n times)
        def multDef(m: Z, n: Z): Z
            //anything multiplied by 0 is just 0
            mult0. ∀ x: Z multDef(x, 0) == 0
            multPos. ∀ x: Z (∀ y: Z y > 0 → multDef(x, y) == x + multDef(x, y-1))
}"""
```

We could use this Logika fact in the postcondition and loop invariant block for a multiplication function as follows:

```text
import org.sireum.logika._

l"""{
    fact
        //defines m * n = m + m + ... + m (n times)
        def multDef(m: Z, n: Z): Z
            //anything multiplied by 0 is just 0
            mult0. A x: Z multDef(x, 0) == 0
            multPos. A x: Z (A y: Z y > 0 -> multDef(x, y) == multDef(x, y-1) + x)
}"""


//want to find: num1 + num1 + ... + num1 (a total of num2 times)
def mult(num1: Z, num2: Z): Z = {
    l"""{
        requires num2 >= 0
        ensures result == multDef(num1, num2)
    }"""

    var answer: Z = 0
    var cur: Z = 0

    while (cur != num2) {
        l"""{
            invariant
                answer == multDef(num1, cur)
                cur >= 0
            modifies cur, answer
        }"""
        cur = cur + 1
        answer = answer + num1
    }

    return answer
}
```

The example above does not include the verification steps to prove the loop invariant and postcondition, but those things could be accomplished in the same way as for the `factorial` function.

## Logika fact for Fibonacci numbers

The Fibonacci sequence is:

```math
$$
1, 1, 2, 3, 5, 8, 13, ...
$$
```

<br><br>

The first two Fibonacci numbers are both 1, and subsequent Fibonacci numbers are found by adding the two previous values. In the sequence above, we see that the two latest numbers were 8 and 13, so the next number in the sequence will be {{< math >}}$8 + 13 = 21${{< /math >}}.

We can recursively define the Fibonacci sequence as follows:

- Base case 1: the first Fibonacci number is 1
- Base case 2: the second Fibonacci number is 1
- Recursive case: for all numbers x greater than 2, the x-th Fibonacci number is the (x-1)-th Fibonacci number + the (x-2)-th Fibonacci number

We can translate this directly to a Logika fact:

```text
l"""{
    fact
        //defines the nth number in the Fibonacci sequence
        //1, 1, 2, 3, 5, 8, 13, ...
        def fibDef(n: Z): Z
            fib1. fibDef(1) == 1
            fib2. fibDef(2) == 1
            fibN. ∀ x: Z x > 2 → fibDef(x) == fibDef(x-1) + fibDef(x-2)
}"""
```

Which we could use in a postcondition and loop invariant if we wrote a function to compute the Fibonacci numbers.