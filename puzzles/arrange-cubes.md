# Arrange Cubes

https://www.interviewbit.com/problems/arrange-cubes/

A man has two cubes on his desk.
Every day he arranges both cubes so that the front faces show the current day of the month. 
What numbers are on the faces of the cubes to allow this?

Sort the digits on cube 1 and sort the digits on cube 2 and append them to give the answer.

For example, if the first cube had digits 1 2 3 4 5 6 and second had digits 3 1 9 8 2 4, 
the sorted digits become :
```
First cube : 1 2 3 4 5 6
Second cube : 1 2 3 4 8 9
```

So, the answer would be "123456123489"


## Solution

We have two cubes, that means 12 faces or 12 numbers one on each face. The all possible dates are 1 to 31, that includes 11 and 22. So 1 and 2 should be there on both cubes. It means we need 12 digits (0-9 and 1, 2). Now if we see the distribution of numbers on each faces, we have 1 and 2 on both cubes. For 30 we need 0 and 3 on different cubes. So lets say, first cube has 1, 2, 3, 4, 5, 6 and other one has 0, 1, 2, 7, 8, 9. It looks fine, but we we notice, the man uses both the cubes for each day, so how do we show 07, 08, 09 ???So that means we need to have 0 on both the cubes, but that makes it 13, but there are only 12 faces. Thats the trick in this question, we can place cubes upside down too, now which is the number we can use both the ways, yes its 6, it can be used as 9 and then we can have all the possible dates.

so first cube : 0, 1, 2, 3, 4, 5

second cube : 0, 1, 2, 6, 7, 8

Answer: 012345012678
