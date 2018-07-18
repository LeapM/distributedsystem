# The Byzantine Generals Problem

THe prolbem is to find an algorithm to ensue that the loyal general will reach agreement. 

**With unforgeable writeen messages, the problem is solvable for any number of generals and possible trators**

## Requirement

1. All loyal generals decide upon the same plan of action

2. A small number of traitors cannot cause the loyal generals to adopt a bad plan.

    compare with the first one,this reqirement needs a robust method to reach agreement. 


## Satisyng condition A requires:

1. any two loyal generals use the same value of v(i),  which implies that a general cannot necessarily use a vlaue of v(i)
obtain direclty from the ith general. 

2. if the ith general is loyal, v(i) must be used by every loyal generals. 


both conditions on the single value sent by the ith general. We can therefore restrict our condisiton to the problem of how a **single generals sends his vlaue to the others**

## rewrite the conditions

1. All loyal lieutenants obey the same order

2. if the commanding general loyal, very loyal lieutenant obeys the order

The conditions are called the interactive consistency conditions

if the general can send **only oral messages** then no solution will work unless more than two thirds of the general are loyal. 

informal approval of one traitor out of three generals are impossible

by contradition, it approved the 3m + 1 generals are required to cope with m trators.

**Reaching approximate agreement is just as hard as reaching exact agreement**
