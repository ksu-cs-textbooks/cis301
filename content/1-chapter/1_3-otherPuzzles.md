---
title: "Other Puzzles"
weight: 22
pre: "1.3. "
---

We will look at a variety of other logic puzzles, each of which involve some statements being false and some statements being true.

## Lion and Unicorn

The setup for a Lion and Unicorn puzzle can vary, but the idea is that both Lion and Unicorn have specific days that they tell only lies, and other specific days that they only tell the truth.

Here is one example:


> Lion always lies on Mondays, Tuesdays, and Wednesdays.<br>
> Lion always tells the truth on other days.<br>
> Unicorn always lies on Thursdays, Fridays, and Saturdays, and always tells the truth on other days. <br><br>
> On Sunday, everyone tells the truth.
<br><br>
> Lion says:  "Yesterday was one of my lying days."<br>
> Unicorn says:  "Yesterday was one of my lying days, too."
<br><br>
> What day is it?

<details>
    <summary> <b> -→ Click for solution </b></summary>

To solve this puzzle, we consider what Lion's and Unicorn's statements would mean on each different day of the week.

- Suppose it is Sunday. Then Lion's statement would be a lie (Lion does not lie on Saturday), and yet Lion is supposed to be telling the truth on Sunday.

- Suppose it is Monday. Then both Lion's and Unicorn's statements would be lies, since they both told the truth yesterday (Sunday).

- Suppose it is either Tuesday or Wednesday. Then Lion's statement would be true -- but Lion is supposed to lie on both Tuesday and Wednesday.

- Suppose it is Thursday. Then Lion's statement would be true (Wednesday was one of their lying days), which is good since Lion is supposed to be telling the truth on Thursdays. Similarly, Unicorn's statement would be false (Unicorn does not lie on Thursdays), which works out since Unicorn DOES lie on Thursdays.

- Suppose it is either Friday or Saturday. Then Lion's statement would be a lie (Lion doesn't lie on either Thursday or Friday), but Lion should be telling the truth on Friday and Saturday.


We can conclude that it must be Thursday.

</details>

## Tweedledee and Tweedledum

The Tweedledee and Tweedledum puzzles originate from *Through the Looking-glass and What Alice Found There*, by Lewis Carroll. There are different versions of these puzzles as well, but all of them involve the identical twin creatures, Tweedledee and Tweedledum. Like with Lion and Unicorn, there are different days on which Tweedledee and Tweedledum either only lie or only tell the truth (and often one creature is lying while the other is telling the truth). 

### Example 1

Consider this puzzle:

> Tweedledee and Tweedledum are identical. You know that one of them lies Mon/Tues/Wed,and that the other lies Thurs/Fri/Sat. (They tell the truth on non-lying days.)
<br><br>
> You don't know which is which.
<br><br>
> You see both of them together. <br>
> The first one says:  "I'm Tweedledum."<br>
> The second one says:  "I'm Tweedledee."
<br><br>
Which is which?  What day is it?

<details>
    <summary> <b> -→ Click for solution </b></summary>

Answer: Since the two creatures gave different answers, we can conclude that they must both be lying or both telling the truth. (Otherwise, both creatures would give you the same name.) Sunday is the only such day.

Each is telling the truth, so the first twin is Tweedledum and the second is Tweedledee.

</details>

### Example 2

Consider a second puzzle, with the same setup as to which days each twin lies and tells the truth.

> You know that either Tweedledum or Tweedledee has lost a rattle. You find it, and want to return it to the correct one. You don't know what day it is, but are sure that it isn't Sunday (so one must be lying and one must be telling the truth).
<br><br>
> The first one says:  "Tweedledee owns the rattle."
<br><br>
> The second one says: "I'm Tweedledee ¬"
<br><br>
> Who gets the rattle?

<details>
    <summary> <b> -→ Click for solution </b></summary>

To solve this puzzle, we can explore the possibilities for each twin lying or telling the truth.

Suppose the first twin is telling the truth. Since it isn't Sunday, we know the second twin must be lying. If the second twin's statement is a lie, then the second is Tweedledum. Since the first twin is telling the truth, then they are Tweedledee (and the owner of the rattle).

Suppose instead that the first twin is lying. Again, since it isn't Sunday, we know the second twin must be telling the truth. This would make the second twin Tweedledee, and the first twin Tweedledum. It would also mean that TweedleDUM owns the rattle (since the first statement is a lie), which is the first twin.

We don't have enough information to determine which twin is which, but it doesn't matter -- in both cases, the first twin is the owner of the rattle.

</details>

## Portia's Caskets

This type of puzzle originates from *The Merchant of Venice*, by William Shakespeare. In the play, Portia's father devised riddles to test potential suitors for his daughter.  

Here is one such puzzle:

> There are three caskets – one gold, one silver, and one lead. One of the caskets contains a portrait (of Portia). Each casket has a message on it, and you know that at most one of the messages is true.
<br><br>
> Gold casket message: "The portrait is in this casket."
> Silver casket message: "The portrait is not in this casket."
> Lead casket message: "The portrait is not in the gold casket."
<br><br>
> Where is the portrait?

<details>
    <summary> <b> -→ Click for solution </b></summary>

To solve this puzzle, we recognize that there are only three possibilities -- the portrait must be in either the gold casket, the silver casket, or the lead casket. We consider the implications of each:

Suppose the portrait is in the gold casket. Then the messages on both the gold and silver caskets would be true. This isn't possible, as we know that at most one of the messages is true.

Suppose instead that the portait is in the silver casket. Then the messages on the gold and silver caskets would be false, and the message on the lead casket would be true. Only one message is true, so this is a possibility.

Finally, suppose the portrait is in the lead casket. Then the messages on all three caskets would be true, so this isn't possible.

We conclude that the portrait must be in the silver casket.

</details>