---
title: "Recursion"
pre: "9.2. "
weight: 101
date: 2018-08-24T10:53:26-05:00
---

In this section, we will see how to prove the correctness of programs that use recursive functions. We will see that verifying a recursive function is exactly the same as verifying a non-recursive function:

- We must prove a function's preconditions before calling it (including before making a recursive call)
- After calling a function, we can list the function's postconditions as premises (including after making a recursive call)
- The function can list its preconditions as premises
- The function must prove its postconditions just before it ends

## Writing a recursive mult function

We know we can multiply two numbers, `x` and `y`, using the `*` operator -- `x * y`. But what if we wanted to find the same result using only addition, not multiplication? Multiplication can be thought of as repeated addition -- `x * y` is really `x + x + ... + x`, where we add together `y` total `x`'s.

We *could* do this repeated addition with a loop (and we will when we introduce loops in section 9.3), but we will use recursion instead. When we write a recursive function, we try to think of two things:

- The *base case*: the simplest version of the problem that we could immediately solve with no more work.
- The *recursive case*: bigger versions of the problem, where we solve a piece of the problem and then recursively solve a smaller piece

In the case of the multiplication `x * y`, we have:

- Base case: if `y` is 0, we have no work to do. Adding togther 0 `x`'s is just 0.
- Recursive case: if `y` is bigger than 0, we do ONE addition (`x + ...`) and recursively add the remaining `y - 1` numbers. (This will become our recursive call.)

With those cases in mind, we can write a recursive `mult` functio:

```text
import org.sireum.logika._

def mult(x: Z, y: Z): Z = {

    var ans: Z = 0

    if (y > 0) {
        var addRest: Z = mult(x, y-1)
        ans = x + addRest
    } else {
        //do nothing
    }

    return ans
}
```

Note that we separated the recursive call (`def addRest: Z = mult(x, y-1)`) from adding on the next piece (`ans = x + addRest`). In Logika, all function calls must go on a separate line by themselves -- we can't combine them with other operations. Also, we included a dummy "else" branch to make the verification simpler. 

## Walking through mult

Suppose we call `mult` as follows:

```text
var times: Z = mult(4, 2)
```

We can trace the recursive calls:

```text
times = mult(4, 2) 
            (x = 4, y = 2)
            addRest = mult(4, 1)    => mult(4, 1)
            ans = 4 + addRest               (x = 4, y = 1)
            returns ans                     addRest = mult(4, 0)    =>  mult(4, 0)
                                            ans = 4 + addRest               (x = 4, y = 0)
                                            returns ans                     ans = 0
                                                                            returns 0
```

We start with `mult(4, 2)`, and then immediately make the recursive call `mult(4, 1)`, which immediately makes the recursive call `mult(4, 0)`. That function instance hits the base case and returns 0. We now return back up the chain of function calls -- the 0 gets returned back to the `mult(4, 1)` instance, which adds 4 and then returns 4:

```text
=> mult(4, 1)
    (x = 4, y = 1)
    addRest = mult(4, 0) = 0
    ans = 4 + addRest = 4 
    returns ans (4)
```

This 4 returns back to the `mult(4, 2)` instance, which adds another 4 and returns 8:

```text
mult(4, 2) 
    (x = 4, y = 2)
    addRest = mult(4, 1) = 4
    ans = 4 + addRest = 8
    returns ans (8)
```

We have now backed our way up the chain -- the 8 is returned back from the original function call, and `times` is set to 8.

## mult function contract

Looking at our `mult` function, we see that the base case is when `y` is 0 and the recursive case is when `y > 0`. Clearly, the function is not intended to work for negative values of `y`. This will be our precondition -- that `y` must be greater than or equal to 0.

Our postcondition should describe what `mult` is returning in terms of its parameters. In this case, we know that `mult` is performing a multiplication of `x` and `y` using repeated addition. So, our function should ensure that it returns `x*y` (that `result == x*y`). Here is the function with the function contract:

```text
import org.sireum.logika._

def mult(x: Z, y: Z): Z = {
    //we still need to add the verification logic blocks

    l"""{
        requires y >= 0
        ensures result == x*y
    }"""

    var ans: Z = 0

    if (y > 0) {
        var addRest: Z = mult(x, y-1)
        ans = x + addRest
    } else {
        //do nothing
    }

    return ans
}
```

## Verification in mult

Now that we have our function contract for `mult`, we must add logic blocks with two things in mind:

- Proving the precondition before a recursive call
- Proving the postcondition before we return from the function

Our recursive call looks like:

```text
var addRest: Z = mult(x, y-1)
```

Since our precondition is `y >= 0`, we see that we must prove that what we are passing as the second parameter (`y-1`, in the case of the recursive call) is greater than or equal to 0. This tells us that before our recursive call, we must have shown exactly: `y-1 >= 0`. We can finish proving the precondition as follows:

```text
import org.sireum.logika._

def mult(x: Z, y: Z): Z = {
    //we still need to prove the postcondition

    l"""{
        requires y >= 0
        ensures result == x*y
    }"""

    var ans: Z = 0

    if (y > 0) {
        l"""{
            1. y > 0                premise     //IF condition is true
            2. y-1 >= 0             algebra 1   //Proves the precondition for the recursive call
        }"""

        var addRest: Z = mult(x, y-1)
        ans = x + addRest
    } else {
        //do nothing
    }

    return ans
}
```

All that remains is to prove the `mult` postcondition -- that we are returning `x*y`. Since we are returning the variable `ans`, then we must prove the claim `ans == x*y` just before our return statement. In order to help with this process, we will need to take advantage of the postcondition after our recursive call. The function promises to return the first parameter times the second parameter, so when we do `addRest: Z = mult(x, y-1)`, we know that `addRest == x*(y-1)` (the first parameter, `x`, times the second parameter, `y-1`). Here is the completed verification

```text
import org.sireum.logika._

def mult(x: Z, y: Z): Z = {
    //verification complete!

    l"""{
        requires y >= 0
        ensures result == x*y
    }"""

    var ans: Z = 0

    if (y > 0) {
        l"""{
            1. y > 0                premise     //IF condition is true
            2. y-1 >= 0             algebra 1   //Proves the precondition for the recursive call
        }"""

        var addRest: Z = mult(x, y-1)

        l"""{
            1. addRest == x*(y-1)   premise     //Postcondition from the recursive call
            2. addRest == x*y - x   algebra 1
        }"""

        ans = x + addRest

        l"""{
            1. addRest == x*y - x   premise     //Pulled from previous block
            2. ans == x + addRest   premise     //From the "ans = x + addRest" assignment statement
            3. ans == x + x*y - x   algebra 1 2
            4. ans == x*y           algebra 3   //Showed the postcondition for the IF branch
        }"""
    } else {
        //do nothing in code - but we still do verification
        //need to show that postcondition will be correct even if we take this branch

        l"""{
            1. ¬(y > 0)             premise     //if condition is false
            2. y >= 0               premise     //precondition
            3. y == 0               algebra 1 2
            4. ans == 0             premise     //ans is unchanged
            5. ans == x*y           algebra 3 4 //Showed the postcondition for the ELSE branch
        }"""
    }

    //Tie together what we learned in both branches
    l"""{
        1. ans == x*y               premise     //shows the postcondition      
    }"""

    return ans
}
```

## Verification of calling code

Verifying the test code that calls a recursive function works exactly the same way as it does for any other function:

- We must prove the precondition before calling the function
- We can list the postcondition as a premise after calling the function

Suppose we want to test `mult` as follows:

```text
val times: Z = mult(4, 2)

assert(times == 8)
```

We could complete the verification by proving the precondition and then using the postcondition to help us prove the claim in the assert:

```text
l"""{
    1. 2 >= 0               algebra     //proves the precondition
}"""

val times: Z = mult(4, 2)

l"""{
    1. times == 4*2         premise     //mult postcondition
    2. times == 8           algebra 1   //needed for the assert
}"""

assert(times == 8)
```

Note that since our second parameter is `2`, that we must demonstrate exactly `2 >= 0` to satisfy `mult`'s precondition. Furthermore, since `mult` promises to return the first parameter times the second parameter, and since we are storing the result of the function call in the `times` variable, then we can claim `times == 4*2` as a premise.