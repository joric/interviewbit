# Cross the Bridge

https://www.interviewbit.com/problems/cross-the-bridge/

Four people are on this side of the bridge. Everyone has to get across. 
Problem is that it’s dark and so you can’t cross the bridge without a flashlight and they only have one flashlight. 
Plus the bridge is only big enough for two people to cross at once. 
The four people walk at different speeds:

1. one fella is so fast it only takes him 1 minute to cross the bridge,
2. another 2 minutes,
3. a third 5 minutes,
4. the last it takes 10 minutes to cross the bridge.

When two people cross the bridge together (sharing the flashlight), they both walk at the slower person’s pace. What is the minimum time required for all 4 to cross the bridge.

Respond with a number which represents the number of minutes.

Answer is a integer. Just put the number without any decimal places if its an integer. If the answer is Infinity, output Infinity. Feel free to get in touch with us if you have any questions 

## Solution
```
1 minutePerson B : 2 minutes
Person C : 5 minutes

Person D : 10 minutes

Let F represents Flashlight.

Person C and D are the slowest guys, if they don’t walk together that it self will make it 15 minutes, In this case (the best way to save time is) :

A,D,F —-Bridge—-> = 10 min

<—-Bridge—-A,F = 1 min (returning with flash light) A,C,F —-Bridge—-> = 5 min

<—-Bridge—-A,F = 1 min A,B,F —-Bridge—-> = 2 min

That makes a total of 19 minutes, but thats not what we want.

It means they both should walk together, in this case if they both are walking together, then there should be another person(because if one of them will bring it will take 5 minutes) to bring the flashlight back. So here is the solution…A,B,F —-Bridge—-> = 2 min

<—-Bridge—-A,F = 1 min C,D,F —-Bridge—-> = 10 min

<—-Bridge—-B,F = 2 min A,B,F —-Bridge—-> = 2 min

Total time = 17 minutes.
```
