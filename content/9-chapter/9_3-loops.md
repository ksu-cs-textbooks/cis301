---
title: "Loops"
pre: "9.3. "
weight: 102
date: 2018-08-24T10:53:26-05:00
---

 A *loop* is a command that restarts itself over and over while its *loop condition* remains true. Loops are trickier to analyze than if/else statements, because we don't know how many times the loop will execute. The loop condition might be initially false, in which case we would skip the body of the loop entirely. Or, the loop might go through 1 iteration, or 10 iterations, or 100 iterations...we don't know. We want a way to analyze what we know to be true after the loop ends, regardless of how many iterations it makes. 

 The only loop available in Logika is a *while* loop, which behaves in the same way as while loops in other languages. If the loop condition is initially false, then the body of the while loop is skipped entirely. If the loop condition is initially true, then we execute the loop body and then recheck the condition. This continues until the loop condition becomes false.

 Here is the syntax of a Scala while loop:

 ```text
while (condition) {
    //body of loop
}
 ```

## Loop invariants

Our solution to analyzing loops despite not knowing the number of iterations is a tool called a *loop invariant*. The job of the loop invariant is to summarize what is always true about the loop. Often, the loop invariant describes the relationship between variables and demonstrates how much progress the loop has made. Sometimes the loop invariant also contains claims that certain variables will always stay in a particular range.

Whatever we choose as the loop invariant, we must be able to do the following:

- Prove the loop invariant is true before the loop begins
- Assume the loop invariant is true at the beginning of an iteration, and prove that the invariant is STILL true at the end of the iteration

## Loop invariants and mathematical induction

The process of proving the correctness of loop invariants is very similar to a mathematical induction proof. We must prove the loop invariant is true before the loop begins, which is analogous to the *base case* in mathematical induction. The process of assuming the invariant is true at the beginning of an iteration and proving that it is still true at the end of an iteration is just like mathematical induction's *inductive step*.

If we prove those two things about our invariant, we can be certain the invariant will still hold after the loop terminates. Why? For the same reason mathematical induction proves a property for a general {{< math >}}$n${{< /math >}}:

- We know the invariant holds before the loop begins
- Because the invariant holds before the loop begins, we are sure it holds at the beginning of the first iteration
- Because we've proved the invariant still holds at the end of each iteration, we're sure it still holds at the end of the first iteration
- Because we're sure it still holds at the end of the first iteration, we know it holds at the beginning of the second iteration
- Because we've proved the invariant still holds at the end of each iteration, we're sure it still holds at the end of the second iteration
...
- We're sure the invariant still holds at the end of each iteration, including the end of the LAST iteration. Thus we're certain the invariant holds just after the loop ends.

## Loop invariant block syntax

In Logika, we will write a *loop invariant block* to describe our loop invariants. This block will go just inside the loop, before the loop body:

```text
while (condition) {
    Invariant(
        Modifies(comma-separated list of variables),
        Invariant_1,
        Invariant_2,
        ...
    )

    //loop body
}
```

Here is a summary of the different loop invariant clauses:

- `Modifies`: uses a comma-separated list to name each variable whose value changes in the loop body
- `Invariant_i`: lists an invariant for the function. If we have multiple invariants, we can list them on separate lines (`Invariant_1`, `Invariant_2`, etc.)


## Example: loop invariant block for a multiplication loop

Suppose we have the following loop to multiply two numbers, `x` and `y`, using repeated addition. (This is very similar to our `mult` function from section 9.2, except it does the additions using a loop instead of using recursion):

```text
// #Sireum #Logika
//@Logika: --manual --background save
import org.sireum._
import org.sireum.justification._

val x: Z = Z.read()
val y: Z = Z.read()

var sum: Z = 0
var count: Z = 0

while (count != y) {
    sum = sum + x
    count = count + 1
}

//now sum is x*y
```

Before writing the loop invariant block, let's make a table showing the values of different variables at different points in the loop:

| Variable | Before loop | After iteration 1 | After iteration 2 | After iteration 3 | ... | After iteration y | 
| --- | --- | --- | --- | --- | --- | --- |
| `count` | 0 | 1 | 2 | 3 | ... | y |
|  `sum` | {{< math >}}$0 (= 0*x)${{< /math >}} | {{< math >}}$x (= 1*x)${{< /math >}} | {{< math >}}$x + x (= 2*x)${{< /math >}} | {{< math >}}$x + x + x (= 3*x)${{< /math >}} | ... | {{< math >}}$x + x + ... + x (= y*x)${{< /math >}} |

Before the loop begins, we've added 0 {{< math >}}$x${{< /math >}}'s together, so the sum is 0. After the first iteration, we've added 1 {{< math >}}$x${{< /math >}} together, so the sum is {{< math >}}$x${{< /math >}}. After the second iteration, we've added 2 {{< math >}}$x${{< /math >}}'s together, so the sum is {{< math >}}$x + x${{< /math >}} which is really {{< math >}}$2 * x${{< /math >}}. This continues until after the y-th iteration, when we've added y {{< math >}}$x${{< /math >}}'s together (and the sum is {{< math >}}$y*x${{< /math >}}). 

Using this table, it is easy to see that at any point, `sum == count*x` (since `count` tracks the number of iterations). This is true both before the loop begins and at the end of each iteration, so it will be our loop invariant.

We now add a loop invariant block to our loop:

```text
// #Sireum #Logika
//@Logika: --manual --background save
import org.sireum._
import org.sireum.justification._

val x: Z = Z.read()
val y: Z = Z.read()

var sum: Z = 0
var count: Z = 0

while (count != y) {
    //loop invariant block (still needs to be proved)
    Invariant(
        Modifies(sum, count),
        sum == count * x
    )

    sum = sum + x
    count = count + 1
}

//now sum is x*y
```

We list `sum` and `count` in the `Modifies` clause because those are the two variables that change value inside the loop.

## Proving the loop invariant

In order to prove the correctness of a loop, we must do two things:

- Prove the loop invariant is true before the loop begins
- Assume the loop invariant is true at the beginning of an iteration, and prove that the invariant is STILL true at the end of the iteration

### Proving loop invariant before loop begins

In our multiplication loop above, let's start by proving the loop invariant before the loop begins. This means that just before the loop, we must prove exactly the claim `sum == count*x`. We can do this with algebra on the current variable values:

```text
// #Sireum #Logika
//@Logika: --manual --background save
import org.sireum._
import org.sireum.justification._

val x: Z = Z.read()
val y: Z = Z.read()

var sum: Z = 0
var count: Z = 0

//prove the invariant before the loop begins
Deduce(
    1 (     sum == 0        )   by Premise,         //from the "sum = 0" assignment
    2 (     count == 0      )   by Premise,         //from the "count = 0" assignment
    3 (     sum == count*x  )   by Algebra*(1, 2)   //proved EXACTLY the loop invariant
)

while (count != y) {
    Invariant(
        Modifies(sum, count),
        sum == count * x
    )

    sum = sum + x
    count = count + 1

    //we still need to prove the invariant after each iteration
}

//now sum is x*y
```

### Proving loop invariant at the end of each iteration

To prove the loop invariant still holds at the end of an iteration, we must have a logic block at the end of the loop body with exactly the claim in the loop invariant (which will now be referring to the updated values of each variable). Since this step has us assuming the loop invariant is true at the beginning of each iteration, we can list the loop invariant as a *premise* in a logic block just inside the loop body. Here is the structure we wish to follow for our multiplication loop:

```text
while (count != y) {
    Invariant(
        Modifies(sum, count),
        sum == count * x
    )

    Deduce(
        1 (     sum == count*x      )   by Premise,     //the loop invariant holds
                                                        //at the beginning of an iteration
    )

    sum = sum + x
    count = count + 1

    //need to prove exactly "sum == count*x"
    //to prove our invariant still holds at the end of an iteration
}
```

We can complete the loop invariant proof by using our tools for processing assignment statements with mutations. Here is the completed verification:

```text
// #Sireum #Logika
//@Logika: --manual --background save
import org.sireum._
import org.sireum.justification._

val x: Z = Z.read()
val y: Z = Z.read()

var sum: Z = 0
var count: Z = 0

//prove the invariant before the loop begins
Deduce(
    1 (     sum == 0        )   by Premise,         //from the "sum = 0" assignment
    2 (     count == 0      )   by Premise,         //from the "count = 0" assignment
    3 (     sum == count*x  )   by Algebra*(1, 2)   //proved EXACTLY the loop invariant
)

while (count != y) {
    Invariant(
        Modifies(sum, count),
        sum == count * x
    )

    Deduce(
        1 (     sum == count*x      )   by Premise,     //the loop invariant holds
                                                        //at the beginning of an iteration
    )

    sum = sum + x

    Deduce(
        1 (     sum == Old(sum) + x     )   by Premise,         //from "sum = sum + x" assignment
        2 (     Old(sum) == count*x     )   by Premise,         //loop invariant WAS true, but sum just changed
        3 (     sum == count*x + x      )   by Algebra*(1,2)    //current knowledge without using Old
    )

    count = count + 1

    Deduce(
        1 (     count == Old(count)+ 1  )   by Premise,         //from "count = count + 1" assignment
        2 (     sum == Old(count)*x + x )   by Premise,         //from previous "sum = count*x + x", 
                                                                //but count has changed
        3 (     sum == (count-1)*x + x  )   by Algebra*(1,2),
        4 (     sum == count*x - x + x  )   by Algebra*(3),
        5 (     sum == count*x          )   by Algebra*(4)      //loop invariant holds at end of iteration
    )
}

//now sum is x*y
```

### Knowledge after loop ends

In the example above, suppose we add the following assert after the loop ends:

```text
assert(sum == x*y)
```

This seems like a reasonable claim -- after all, we said that our loop was supposed to calculated `x * y` using repeated addition. We have proved the loop invariant, so we can be sure that `sum == count*x` after the loop...but that's not quite the same thing. Does `count` equal `y`? How do we know?

We can prove our assert statement by considering one more piece of information -- if we have exited the loop, we know that the loop condition must be false. In fact, you always know two things (which you can claim as premises) after the loop ends:

1. The loop condition is false (so we can claim `¬(condition)`)
2. The loop invariant is true, since we proved is true at the end of each iteration

We can use those pieces of information to prove our assert statement:

```text
//the multiplication loop example goes here

Deduce(
    1 (     sum == count*x  )   by Premise,     //the loop invariant holds
    2 (     ¬(count != y)   )   by Premise,     //the loop condition is not true
    3 (     count == y      )   by Algebra*(2),
    4 (     sum == x*y      )   by Algebra*1,3  //proves our assert statement
)

assert(sum == x*y)
```

## Functions with loops

If we have a function that includes a loop, we must do the following:

- Prove the loop invariant is true before the loop begins
- Given that the loop invariant is true at the beginning of an iteration, prove that it is still true at the end of the iteration
- Use the combination of the loop invariant and the negation of the loop condition to prove the postcondition

For example, suppose our loop multiplication is inside a function which is tested by some calling code. We would like to add a function contract, our loop invariant proof, and necessary logic blocks to show that our assert holds at the end of the calling code. Here is just the code for the example:

```text
// #Sireum #Logika
//@Logika: --manual --background save
import org.sireum._
import org.sireum.justification._

def mult(x: Z, y: Z) : Z = {
    var sum: Z = 0
    var count: Z = 0

    while (count != y) {
        sum = sum + x
        count = count + 1
    }

    return sum
}

//////////// Test code //////////////

var one: Z = 3
var two: Z = 4

var answer: Z = mult(one, two)
assert(answer == 12)
```

We start by adding a function contract to `mult`, which will be the same as our function contract for the recursive version of this function in section 9.2 -- `y` needs to be nonnegative, and we promise to return `x*y`. Here is the code after adding the function contract and our previous loop verificaton:

```text
// #Sireum #Logika
//@Logika: --manual --background save
import org.sireum._
import org.sireum.justification._

def mult(x: Z, y: Z) : Z = {
    //function contract
    Contract(
        Requires(   y >= 0          ),  //precondition: y should be nonnegative
        Ensures(    Res[Z] == x * y )   //postcondition (we promise to return x*y)
    )

    var sum: Z = 0
    var count: Z = 0

    //prove the invariant before the loop begins
    Deduce(
    1 (     sum == 0        )   by Premise,         //from the "sum = 0" assignment
    2 (     count == 0      )   by Premise,         //from the "count = 0" assignment
    3 (     sum == count*x  )   by Algebra*(1, 2)   //proved EXACTLY the loop invariant
)

    while (count != y) {
        Invariant(
            Modifies(sum, count),
            sum == count * x
        )

        Deduce(
            1 (     sum == count*x      )   by Premise,     //the loop invariant holds
                                                            //at the beginning of an iteration
        )

        sum = sum + x

        Deduce(
            1 (     sum == Old(sum) + x     )   by Premise,         //from "sum = sum + x" assignment
            2 (     Old(sum) == count*x     )   by Premise,         //loop invariant WAS true, but sum just changed
            3 (     sum == count*x + x      )   by Algebra*(1,2)    //current knowledge without using Old
        )

        count = count + 1

        Deduce(
            1 (     count == Old(count)+ 1  )   by Premise,         //from "count = count + 1" assignment
            2 (     sum == Old(count)*x + x )   by Premise,         //from previous "sum = count*x + x", 
                                                                    //but count has changed
            3 (     sum == (count-1)*x + x  )   by Algebra*(1,2),
            4 (     sum == count*x - x + x  )   by Algebra*(3),
            5 (     sum == count*x          )   by Algebra*(4)      //loop invariant holds at end of iteration
        )
    }

    //STILL NEED TO PROVE POSTCONDITION

    return sum
}

//////////// Test code //////////////

var one: Z = 3
var two: Z = 4

//STILL NEED TO ADD VERIFICATION FOR ASSERT

var answer: Z = mult(one, two)
assert(answer == 12)
```

We can use the negation of the loop condition (`¬(count != y)`) together with the loop invariant to prove the postcondition will hold before the function returns. We can also apply the same process as in sections 9.1 and 9.2 to prove the precondition in the calling code before calling the `mult` function, and to use the function's postcondition after the call to `mult` to prove our goal assert. Here is the completed example: 

```text
// #Sireum #Logika
//@Logika: --manual --background save
import org.sireum._
import org.sireum.justification._

def mult(x: Z, y: Z) : Z = {
    //function contract
    Contract(
        Requires(   y >= 0          ),  //precondition: y should be nonnegative
        Ensures(    Res[Z] == x * y )   //postcondition (we promise to return x*y)
    )

    var sum: Z = 0
    var count: Z = 0

    //prove the invariant before the loop begins
    Deduce(
    1 (     sum == 0        )   by Premise,         //from the "sum = 0" assignment
    2 (     count == 0      )   by Premise,         //from the "count = 0" assignment
    3 (     sum == count*x  )   by Algebra*(1, 2)   //proved EXACTLY the loop invariant
)

    while (count != y) {
        Invariant(
            Modifies(sum, count),
            sum == count * x
        )

        Deduce(
            1 (     sum == count*x      )   by Premise,     //the loop invariant holds
                                                            //at the beginning of an iteration
        )

        sum = sum + x

        Deduce(
            1 (     sum == Old(sum) + x     )   by Premise,         //from "sum = sum + x" assignment
            2 (     Old(sum) == count*x     )   by Premise,         //loop invariant WAS true, but sum just changed
            3 (     sum == count*x + x      )   by Algebra*(1,2)    //current knowledge without using Old
        )

        count = count + 1

        Deduce(
            1 (     count == Old(count)+ 1  )   by Premise,         //from "count = count + 1" assignment
            2 (     sum == Old(count)*x + x )   by Premise,         //from previous "sum = count*x + x", 
                                                                    //but count has changed
            3 (     sum == (count-1)*x + x  )   by Algebra*(1,2),
            4 (     sum == count*x - x + x  )   by Algebra*(3),
            5 (     sum == count*x          )   by Algebra*(4)      //loop invariant holds at end of iteration
        )
    }

    Deduce(
        1 (     ¬(count != y)       )   by Premise,         //loop condition is now false
        2 (     sum == count*x      )   by Premise,         //loop invariant holds after loop
        3 (     count == y          )   by Algebra*(1),
        4 (     sum == x*y          )   by Algebra*(2,3)    //proves the postcondition
    )

    return sum
}

//////////// Test code //////////////

var one: Z = 3
var two: Z = 4

Deduce(
    1 (     two == 4        )   by Premise,     //from the "two = 4" assignment
    2 (     two >= 0        )   by Algebra*(1)  //proves the mult precondition
)

var answer: Z = mult(one, two)

Deduce(
    1 (     one == 3            )   by Premise,
    2 (     two == 4            )   by Premise,
    3 (     answer == one*two   )   by Premise          //from the mult postcondition
    4 (     answer == 12        )   by Algebra*(1,2,3)  //proves the assert 
)

assert(answer == 12)
```

## How to construct a loop invariant

The most difficult part of the entire process of proving the correctness of a function with a loop is coming up with an appropriate loop invariant. In this section, we will study two additional loops and learn techniques for deriving loop invariants. In general, we need to think about what the loop is doing as it iterates, and what progress it has made so far towards its goal. A good first approach is to trace through the values of variables for several iterations of the loop, as we did with `mult` above -- this helps us identify patterns that can then become the loop invariant.

### Example 1: Sum of odds

Suppose `n` has already been declared and initialized, and that we have this loop:

```text
var total: Z = 0
var i: Z = 0
while (i < n) {
    i = i + 1
    total = total + (2*i - 1)
}
```

It might be difficult to tell what this code is doing before walking through a few iterations. Let's make a table showing the values of different variables at different points in the loop:

| Variable | Before loop | After iteration 1 | After iteration 2 | After iteration 3 | ... | After iteration n | 
| --- | --- | --- | --- | --- | --- | --- |
| `i` | 0 | 1 | 2 | 3 | ... | n |
|  `total` | {{< math >}}$0 ${{< /math >}}| {{< math >}}$1 ${{< /math >}} | {{< math >}}$1 + 3 (= 4)${{< /math >}} | {{< math >}}$1 + 3 + 5 (= 9)${{< /math >}} | ... | {{< math >}}$1 + 3 + 5 + ... + (2*n-1) (=n^2)${{< /math >}} |

Now we can see the pattern -- we are adding up the first {{< math >}}$n${{< /math >}} odd numbers. We can see that at the end of the i-th iteration we have added the first {{< math >}}$i${{< /math >}} odd numbers, where {{< math >}}$(2*i-1)${{< /math >}} is our most recent odd number. We also see that the sum of the first 1 odd number is 1, the sum of the first 2 odd numbers is {{< math >}}$2^2 = 4${{< /math >}}, ..., and the sum of the first {{< math >}}$n${{< /math >}} odd numbers is {{< math >}}$n^2${{< /math >}}.

Since our loop invariant should describe what progress it has made towards its goal of adding the first {{< math >}}$n${{< /math >}} odd numbers, we can see that the loop invariant should be that at any point (before the loop begins and at the end each iteration), {{< math >}}$total${{< /math >}} holds the sum of the first {{< math >}}$i${{< /math >}} numbers (whose value is {{< math >}}$i^2${{< /math >}}). We first try this as our loop invariant:

```text
var total: Z = 0
var i: Z = 0
while (i < n) {
    Invariant(
        Modifies(i, n),
        total == i*i
    )

    i = i + 1
    total = total + (2*i - 1)
}
```

Another consideration of writing a loop invariant is that we should be able to deduce our overall goal once the loop ends. After our loop, we want to be sure that `total` holds the sum of the first `n` odd numbers -- i.e., that `total == n*n`. Our loop invariant tells us that `total == i*i` after the loop ends -- but does `n` necessarily equal `i`?

The way it is written, we can't be certain. We do know that the loop condition must be false after the loop, or that `¬(i < n)`. But this is equivalent to `i >= n` and not `i == n`. We need to tighten our invariant to add a restriction that `i` always be less than or equal to n:

```text
var total: Z = 0
var i: Z = 0
while (i < n) {
    Invariant(
        Modifies(i, n),
        total == i*i,
        i <= n
    )
    
    i = i + 1
    total = total + (2*i - 1)
}
```

After the loop ends, we can now combine the negation of the loop condition, `¬(i < n)` together with the `i <= n` portion of the invariant to deduce that `i == n`. Together with the other half of the invariant -- `total == i*i` -- we can be sure that `total == n*n` when the loop ends.

### Example 2: factorial

Suppose `n` has already been declared and initialized, and that we have this loop:

```text
var prod: Z = 1
var i: Z = 1
while (i != n) {
    i = i + 1
    prod = prod * i
}
```

As before, let's make a table showing the values of different variables at different points in the loop:

| Variable | Before loop | After iteration 1 | After iteration 2 | ... | After iteration n | 
| --- | --- | --- |  --- | --- | --- |
| `i` | 1 | 2 | 3 | ... | n |
|  `prod` | {{< math >}}$1 ${{< /math >}} | {{< math >}}$1 * 2${{< /math >}} | {{< math >}}$1 * 2 * 3${{< /math >}} | ... | {{< math >}}$1 * 2 * 3 * ... * n${{< /math >}} |

From this table, we can clearly see that after {{< math >}}$i${{< /math >}} iterations, {{< math >}}$prod == i!${{< /math >}} (i factorial). This *should* be our loop invariant...but as with many other languages, Logika does not recognize `!` as a factorial operator (instead, it is a negation operator). In the next section, we will see how to create a *Logika fact* to help define the factorial operation. We will then be able to use that Logika fact in place of `!` to let our invariant be a formalization of: *prod equals i factorial*.