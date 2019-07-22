# Bulbs

https://www.interviewbit.com/problems/bulbs

N light bulbs are connected by a wire. Each bulb has a switch associated with it,
however due to faulty wiring, a switch also changes the state of all the bulbs
to the right of current bulb. Given an initial state of all bulbs, find the minimum
number of switches you have to press to turn on all the bulbs. You can press the same switch multiple times.

Note: 0 represents the bulb is off and 1 represents the bulb is on.

Example:

Input: [0 1 0 1]
Return: 4

Explanation :
    press switch 0: [1 0 1 0]
    press switch 1: [1 1 0 1]
    press switch 2: [1 1 1 0]
    press switch 3: [1 1 1 1]



## Hint 1

You will never need to press the same switch twice.
Why? Because it is equivalent to not pressing the switch and you will end up with the same state as before.
So we can always solve the problem in at most n switch flips.

## Hint 2

The order in which you press the switch does not affect the final state.

Example:

Input: [0 1 0 1]

Case 1:

Press switch 0: [1 0 1 0]

Press switch 1: [1 1 0 1]

Case 2:

Press switch 1: [0 0 1 0]

Press switch 0: [1 1 0 1]

Therefore we can choose a particular order.
To make things easier lets go from left to right.
At the current position if the bulb is on we move to the right,
else we switch it on. This works because changing any switch to
the right of it will not affect it anymore.




## Solution

```cpp

int Solution::bulbs(vector<int> &A) {
    int state = 0, ans = 0;
    for (int i = 0; i < A.size(); i++) {
        if (A[i] == state) {
            ans++;
            state = 1 - state;
        }
    }
    return ans;
}


int Solution::bulbs(vector<int> &A) {

    int count = 0;
    int orig = 1;

    for (int i = 0; i < A.size(); i++) {
        if (A[i] == 0) {
            if (orig == 1) {
                count++;
                orig = 0;
            }
        } else {
            if (orig == 0) {
                count++;
                orig = 1;
            }
        }
    }

    return count;
}
```


