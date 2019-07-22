# Assign Mice To Holes

https://www.interviewbit.com/problems/assign-mice-to-holes




There are N Mice and N holes are placed in a straight line. 
Each hole can accomodate only 1 mouse. 
A mouse can stay at his position, move one step right from x to x + 1, or move one step left from x to x âˆ' 1. Any of these moves consumes 1 minute.
Assign mice to holes so that the time when the last mouse gets inside a hole is minimized.

Example:

positions of mice are:
4 -4 2
positions of holes are:
4 0 5

Assign mouse at position x=4 to hole at position x=4: Time taken is 0 minutes 
Assign mouse at position x=-4 to hole at position x=0: Time taken is 4 minutes 
Assign mouse at position x=2 to hole at position x=5: Time taken is 3 minutes 
After 4 minutes all of the mice are in the holes.

Since, there is no combination possible where the last mouse's time is less than 4, 
answer = 4.
Input:

A:  list of positions of mice
B:  list of positions of holes
Output:

single integer value
 NOTE: The final answer will fit in a 32 bit signed integer. 



## Hint 1

Hint: Sorting

Can some greedy approach work here ?

## Hint 2

## Solution Approach:

sort mice positions (in any order)
sort hole positions 

Loop i = 1 to N:
    update ans according to the value of |mice(i) - hole(i)|

Proof of correctness:

Let i1 < i2 be the positions of two mice and let j1 < j2 be the positions of two holes. 
It suffices to show via case analysis that

max(|i1 - j1|, |i2 - j2|) <= max(|i1 - j2|, |i2 - j1|) , 
    where '|a - b|' represent absolute value of (a - b)
since it follows by induction that every assignment can be transformed by a series of swaps into the sorted assignment, where none of these swaps increases the makespan


## Solution

```cpp

////////////////
// editorial

int Solution::mice(vector<int> &mice, vector<int> &hole) {
    int N = mice.size();
    sort(mice.begin(), mice.end());
    sort(hole.begin(), hole.end());

    int ans = 0;
    for (int i = 0; i < N; ++i) {
        ans = max(ans, abs(mice[i] - hole[i]));
    }
    return ans;
}

////////////////

int Solution::mice(vector<int> &A, vector<int> &B) {
    if (A.size() == 0) {
        return 0;
    }

    sort(A.begin(), A.end());
    sort(B.begin(), B.end());

    int i = 0, j = 0;
    int sol = INT_MIN;

    while (i != A.size()) {
        sol = max(abs(A[i] - B[j]), sol);
        i++;
        j++;
        if (j == B.size()) {
            j--;
        }
    }

    return sol;
}
```