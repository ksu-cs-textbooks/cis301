---
title: "Propositional Atoms"
pre: "3.1. "
weight: 40
date: 2018-08-24T10:53:26-05:00
---

## Definition

A *propositional atom* is statement that is either true or false, and that contains no logical connectives (like and, or, not, if/then).

## Examples of propositional atoms

For example, the following are propositional atoms:

- My shirt is red.
- It is sunny.
- Pigs can fly.
- I studied for the test.

## Examples of what are NOT propositional atoms

Propositional atoms should not contain any logical connectives. If they did, this would mean would could have further subdivided the statement into multiple propositional atoms that could be joined with logical operators. For example, the following are NOT propositional atoms:

- It is not summer. (*contains a not*)
- Bob has brown hair and brown eyes. (*contains an and*)
- I walk to school unless it rains. (*contains the word `unless`, which has if...then information*)

Propositional atoms also must be either true or false -- they cannot be questions, commands, or sentence fragments. For example, the following are NOT propositional atoms:

- What time is it? (*contains a question - not a true/false statement*)
- Go to the front of the line. (*contains a command - not a true/false statement*)
- Fluffy cats (*contains a sentence fragment - not a true/false statement*)

## Identifying propositional atoms

If we are given several sentences, we identify its propositional atoms by finding the key statements that can be either true or false. We further ensure that these statements do not contain any logical connectives (and, or, not, if/then information) - if they do, we break the statement down further. We then assign letters to each proposition.

For example, if we have the sentences:

```text
My jacket is red and green. I only wear my jacket when it is snowing. It did not snow today.
```

Then we identify the following propositional atoms:

```text
p: My jacket is red
q: My jacket is green
r: I wear my jacket
s: It is snowing
t: It snowed today
```

Notice that the first sentence, "My jacket is red and green", contained the logical connective "and". Thus, we broke that idea into its components, and got propositions `p` and `q`. The second sentence, "I only wear my jacket when it is snowing", contained if/then information about when I would wear my jacket. We broke that sentence into two parts as well, and got propositions `r` and `s`. Finally, the last sentence, "It did not snow today", contained the logical connective "not" -- so we removed it and kept the remaining information for proposition `t`. 

Each propositional atom is a true/false statement, just as is required.

In the next section, we will see how to complete our translation from English to propositional logic by connecting our propositional atoms with logical operators.