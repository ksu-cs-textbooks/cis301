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
@spec def proofFunction(paramList): returnType = $

@spec def proofFacts = Fact(
    proofFunction(baseCase) == baseValue,
    ...
    ∀( (x: Z) => (rangeOfX) → (proofFunction(x) == proofFunction(x - 1) * x) )
)
```

In the above definition, `proofFunction` is the name we give our proof function, `paramList` is the list of parameter names and types needed for this proof function (which are formatted exactly like a parameter list in a regular Logika function), and `returnType` is the return type of the proof function (usually either `Z` for integer or `B` for boolean). The `= $` at the end of the proof function is indicating that its definition will be provided later.

Below the proof function, we include the *proof facts*, which is a recursive definition of the values for our proof function. We include one or more base cases, which list the value of our proof function for its smallest possible input (or for the smallest several inputs). Finally, we include our recursive case as a quantified statement -- it lists the value of our proof function on all inputs bigger than our base cases. This recursive case uses the proof function's definition for a smaller value, like `proofFunction(x-1)`. 

Logika facts are defined at the top of the Logika file, below the `import`s but before any of the code.

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
@spec def factFunction(n: Z): Z = $

@spec def factorialFacts = Fact(
    factFunction(1) == 1,
    ∀ ( (x: Z) => (x > 1) → (factFunction(x) == factFunction(x-1)*x) )
)
```

Let's consider each portion of this proof function. Here, `factDef` is the name given to the proof function. It takes one parameter, `n`, which is an integer, and it returns an integer. We have two possible ways of calculating the value for the proof function, which we detail in `factorialFacts`. First, we define our base case:

```text
factFunction(1) == 1
```

Which says that `factFunction(n)` is 1 if {{< math >}}$n == 1${{< /math >}}. This is the same as our base case in our recursive definition for factorial -- {{< math >}}$1! = 1${{< /math >}}.

Next, consider the recursive case of our proof function:

```text
∀ ( (x: Z) => (x > 1) → (factFunction(x) == factFunction(x-1)*x) )
```

This case states that for all integers `x` that are bigger than 1, we define `factFunction(x) == x * factFunction(x - 1)`. This is the same as our recursive case in our recursive definition for factorial -- for values of {{< math >}}$n${{< /math >}} greater than 1, {{< math >}}$n! = n * (n-1)!${{< /math >}}.

## Evaluating a Logika fact

Suppose we used our `factorialFacts` proof function to calculate `factFunction(3)`. We would have:

```text
factFunction(3) == 3 * factFunction(2)      //we use the recursive case, since 3 > 1
factFunction(2) == 2 * factFunction(1)      //we use the recursive case, since 2 > 1
factFunction(1) == 1                        //we use the base case       
```

Once we work down to:

```text
factFunction(1) == 1
```
 
We can plug `1` in for `facfactFunctiontDef(1)` in `factFunction(2) == 2 * factFunction(1)`, which gives us:

```text
factFunction(2) == 2
```

Similarly, we can plug `2` in for `factFunction(2)` in `factFunction(3) == 3 * factFunction(2)`, and see that:

```text
factFunction(3) == 6
```

## Using Logika facts as justifications

If we had our proof function, `factFunction`, then we could pull its two facts from its `factorialFacts` recursive definition into a proof block like this:

```text
Deduce(
    1 (     factFunction(1) == 1                                                )   by ClaimOf(factorialFacts _),                                             
    2 (     ∀ ( (x: Z) => (x > 1) → (factFunction(x) == factFunction(x-1)*x) )  )   by ClaimOf(factorialFacts _)
)
```

Note that we must pull in the definitions EXACTLY as they are written in the proof function. The justification is always `ClaimOf(proofFacts _)` where `proofFacts` is the name of the recursive definition.

## Using Logika facts in postconditions and invariants

Consider the following full Logika program that includes a function to find and return a factorial, as well as test code that calls our factorial function:

```text
// #Sireum #Logika
//@Logika: --manual --background save
import org.sireum._
import org.sireum.justification._
import org.sireum.justification.natded.prop._
import org.sireum.justification.natded.pred._

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

We want our `factorial` function contract to say that it is returning `n!`, and that it is only defined for values of `n` that are greater than or equal to 0. We recall that our Logika fact, `factFunction`, defines the factorial operation:

```text
@spec def factFunction(n: Z): Z = $

@spec def factorialFacts = Fact(
    factFunction(1) == 1,
    ∀ ( (x: Z) => (x > 1) → (factFunction(x) == factFunction(x-1)*x) )
)
```

And we will use `factFunction` to define what we mean by "factorial" in our `factorial` function contract:

```text
def factorial(n: Z): Z = {
    Contract(
        Requires(   n >= 1                      ),  //factFunction(n) is only defined when n >= 1
        Ensures(    Res[Z] == factFunction(n)   )   //we promise to return factFunction(n), where factFunction defines n!
    )

    //code for factorial function
}
```

### Writing a loop invariant block using a Logika fact

We can similarly use `factFunction` in our loop invariant block. We noted at the end of section 9.3 that the invariant in our loop should be: *prod equals i factorial*. Now we have a way to express what we mean by "factorial", so our invariant will be: `prod == factFunction(i)`. Since the `factFunction` proof function is only defined for parameters greater than or equal to 1, we need to add a second piece to the invariant to guarantee that `i` will always be greater than or equal to 1. We now have the following loop invariant block:

```text
while (i != n) {
    Invariant(
        Modifies(i, product),
        product == factFunction(i),
        i >= 1
    )

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
// #Sireum #Logika
//@Logika: --manual --background save
import org.sireum._
import org.sireum.justification._
import org.sireum.justification.natded.prop._
import org.sireum.justification.natded.pred._

@spec def factFunction(n: Z): Z = $

@spec def factorialFacts = Fact(
    factFunction(1) == 1,
    ∀((x: Z) => (x > 1) → (factFunction(x) == factFunction(x-1)*x))
)


def factorial(n: Z): Z = {
    Contract(
        Requires(   n >= 1                      ),  //factFunction(n) is only defined when n >= 1
        Ensures(    Res[Z] == factFunction(n)   )   //we promise to return factFunction(n), where factFunction defines n!
    )

    var i: Z = 1        //how many multiplications we have done
    var product: Z = 1  //my current calculation

    //Prove invariant before loop begins
    Deduce(
        1 (     i == 1                      ) by Premise,
        2 (     product == 1                ) by Premise,

         //pull in proof function base case
        3 (     factFunction(1) == 1        ) by ClaimOf(factorialFacts _),

        //proves first loop invariant holds 
        4 (     product == factFunction(i)  ) by Algebra*(1, 2, 3),

        //proves second loop invariant holds 
        5 (     i >= 1                      ) by Algebra*(1)
    )

    while (i != n) {
        Invariant(
            Modifies(i, product),
            product == factFunction(i),
            i >= 1
        )

        i = i + 1

        Deduce(
            //from "i = i + 1"
            1 (     i == Old(i) + 1                  ) by Premise,

            //loop invariant held before changing i
            2 (     product == factFunction(Old(i))  ) by Premise,

            //rewrite invariant with no "Old"
            3 (     product == factFunction(i - 1)   ) by Algebra*(1, 2),

            //second loop invariant held before changing i
            4 (     Old(i) >= 1                      ) by Premise,

            //needed for the Logika fact
            5 (     i > 1                            ) by Algebra*(1, 4)
        )

        product = product * i

        //Prove invariant still holds at end of iteration
        Deduce(
            //from "product = product * i"
            1 (  product == Old(product) * i                                        ) by Premise,

            //from previous logic block
            2 (  Old(product) == factFunction(i - 1)                                ) by Premise,

             //pull in recursive case from proof function
            3 (  ∀( (x: Z) => x > 1 → factFunction(x) == factFunction(x - 1) * x )  ) by ClaimOf(factorialFacts _),

            //plug in "i" for "x" (where i is of type Z)
            4 (  i > 1 → factFunction(i) == factFunction(i - 1) * i                 ) by AllE[Z](3),

            //from previous logic block
            5 (  i > 1                                                              ) by Premise,

            //i > 1, so get right side of →
            6 (  factFunction(i) == factFunction(i - 1) * i                         ) by ImplyE(4, 5),
            7 (  product == factFunction(i - 1) * i                                 ) by Algebra*(1, 2),

            //proves first invariant still holds
            8 (  product == factFunction(i)                                         ) by Algebra*(6, 7),

            //proves second invariant still holds
            9 (  i >= 1                                                             ) by Algebra*(5)
        )
    }

    //Prove postcondition
    Deduce(
        1 (     product == factFunction(i)  ) by Premise,      //loop invariant
        2 (     !(i != n)                   ) by Premise,      //loop condition false
        3 (     i == n                      ) by Algebra*(2),
        4 (     product == factFunction(n)  ) by Algebra*(1, 3)
    )

    return product
}

//////// Test code ///////////

var num: Z = 2

//Prove precondition
Deduce(
    1 (     num == 2  ) by Premise,
    2 (     num >= 1  ) by Algebra*(1)     //proves factorial precondition
)

var answer: Z = factorial(num)

Deduce(
    1 (  answer == factFunction(num)                                        ) by Premise,       //factorial postcondition
    2 (  num == 2                                                           ) by Premise,
    3 (  answer == factFunction(2)                                          ) by Algebra*(1, 2),

    //pull in recursive case from proof function
    4 (  ∀( (x: Z) => x > 1 → factFunction(x) == factFunction(x - 1) * x )  ) by ClaimOf(factorialFacts _),

     //plug in "2" for "x" (where 2 is an integer of type Z)
    5 (  2 > 1 → factFunction(2) == factFunction(2 - 1) * 2                 ) by AllE[Z](4),
    6 (  2 > 1                                                              ) by Algebra*(),

    //2 > 1, so use →
    7 (  factFunction(2) == factFunction(2 - 1) * 2                         ) by ImplyE(5, 6),

    //pull in base case from proof function
    8 (  factFunction(1) == 1                                               ) by ClaimOf(factorialFacts _),
    9 (  factFunction(2) == factFunction(1) * 2                             ) by Algebra*(7),
    10 (  factFunction(2) == 2                                              ) by Algebra*(8, 9),

    //proves claim in assert
    11 (  answer == 2                                                       ) by Algebra*(1, 2, 10)
)

assert(answer == 2)
```

## Logika fact for multiplication

Suppose we wanted to create a Logika fact that recursively defined multiplication. We first recursively define how we would multiply {{< math >}}$x * y${{< /math >}}. We know that our base case will be when {{< math >}}$y == 0${{< /math >}}, because anything times 0 is 0. We also saw that multiplication can be defined as repeated addition, so that {{< math >}}$x * y == x + x + ... x${{< /math >}} for a total of {{< math >}}$y${{< /math >}} additions. We also see that {{< math >}}$x * y == x + x * (y-1)${{< /math >}}, since we can pull out one of the additions and then have {{< math >}}$y-1${{< /math >}} additions left to do. 

Here is our recursive definition of the problem:

- Base case: for all numbers x, x * 0 is 0
- Recursive case: for all numbers x and all positive numbers y, x * y = x + x * (y-1)

We can translate this directly to a proof function:

```text
@spec def multFunction(m: Z, n: Z): Z = $

@spec def multFacts = Fact(
    //anything multiplied by 0 is just 0
    ∀((x: Z) => multFunction(x, 0) == 0 ),

    //defines m * n = m + m + ... + m (n times)
    ∀( (x: Z) => ∀( (y: Z) => (y > 1) → (multFunction(x, y) == multFunction(x,y-1)) ) )
)
```

We could use this Logika fact in the postcondition and loop invariant block for a multiplication function as follows:

```text
// #Sireum #Logika
//@Logika: --manual --background save
import org.sireum._
import org.sireum.justification._
import org.sireum.justification.natded.prop._
import org.sireum.justification.natded.pred._

@spec def multFunction(m: Z, n: Z): Z = $

@spec def multFacts = Fact(
    //anything multiplied by 0 is just 0
    ∀((x: Z) => multFunction(x, 0) == 0 ),

    //defines m * n = m + m + ... + m (n times)
    ∀( (x: Z) => ∀( (y: Z) => (y > 1) → (multFunction(x, y) == multFunction(x,y-1)) ) )
)


//want to find: num1 + num1 + ... + num1 (a total of num2 times)
def mult(num1: Z, num2: Z): Z = {
    Contract( 
        Requires( num2 >= 1 ),
        Ensures( Res[Z] == multFunction(num1, num2) )
    )
  
    var answer: Z = 0
    var cur: Z = 0

    while (cur != num2) {
        Invariant(
            Modifies(cur, answer),
            answer == multFunction(num1, cur),
            cur >= 0
        )

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
//defines the nth number in the Fibonacci sequence
//1, 1, 2, 3, 5, 8, 13, ...
@spec def fibFunction(n: Z): Z = $

@spec def fibFacts = Fact(
    fibFunction(1) == 1,
    fibFunction(2) == 2,
    ∀( (x: Z) => (x > 2) → fibFunction(x-1) + fibFunction(x-2) )

    //defines m * n = m + m + ... + m (n times)
    ∀( (x: Z) => ∀( (y: Z) => (y > 1) → (multFunction(x, y) == multFunction(x,y-1)) ) )
)
```

Which we could use in a postcondition and loop invariant if we wrote a function to compute the Fibonacci numbers.