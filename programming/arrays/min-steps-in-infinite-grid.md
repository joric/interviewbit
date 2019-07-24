# Min Steps in Infinite Grid

https://www.interviewbit.com/problems/min-steps-in-infinite-grid/

## Description

You are in an infinite 2D grid where you can move in any of the 8 directions :

```
(x,y) to
  (x+1, y),
  (x-1, y),
  (x, y+1),
  (x, y-1),
  (x-1, y-1),
  (x+1,y+1),
  (x-1,y+1),
  (x+1,y-1)
```

You are given a sequence of points and **the order in which you need to cover the points**.
Give the minimum number of steps in which you can achieve it. You start from the first point.

### Input

Given two integer arrays A and B, where A[i] is x coordinate and B[i] is y coordinate of ith point respectively.

### Output

Return an Integer, i.e minimum number of steps.

### Example

```
Input : [(0, 0), (1, 1), (1, 2)]
Output : 2
```

It takes 1 step to move from (0, 0) to (1, 1). It takes one more step to move from (1, 1) to (1, 2).

### Note

This question is intentionally left slightly vague. Clarify the question by trying out a few cases.

## Hint 1

Note that because the order of covering the points is already defined, the problem just reduces to figuring out the way to calculate the distance between 2 points (A, B) and (C, D).
Can you think of a formula for calculating the distance in O(1) ?

## Solution Approach

Note that because the order of covering the points is already defined, the problem just reduces to figuring out the way to calculate the distance between 2 points (A, B) and (C, D).

Note that what only matters is X = abs(A-C) and Y = abs(B-D).

While X and Y are positive, you will move along the diagonal and X and Y would both reduce by 1. 
When one of them becomes 0, you would move so that in each step the remaining number reduces by 1.

In other words, the total number of steps would correspond to max(X, Y).
Take a few examples to convince yourself.

## Solution

### Editorial
```cpp
int Solution::coverPoints(vector<int> x, vector<int> y) {
    if (x.size() <= 1) return 0;
    assert(x.size() == y.size());
    int ans = 0;
    for (int i = 1; i < x.size(); i++) {
        ans += max(abs(x[i] - x[i-1]), abs(y[i] - y[i-1]));
    }
    return ans;
}
```

### Fastest
```cpp
int Solution::coverPoints(vector<int> &A, vector<int> &B) {
    int n, a, b, distance = 0;
    for (n = 1; n < A.size(); n++) {
        a = (A[n] > A[n - 1]) ? A[n] - A[n - 1] : A[n - 1] - A[n];
        b = (B[n] > B[n - 1]) ? B[n] - B[n - 1] : B[n - 1] - B[n];
        if (a > b)
            distance += a;
        else
            distance += b;
    }
    return distance;
}
```

### Lightweight
```cpp
int Solution::coverPoints(vector<int> &A, vector<int> &B) {
    int count = A.size();
    int i = 0;
    int steps = 0, diff = 0;
    for (i = 1; i < count; i++) {
        if (abs(A[i] - A[i - 1]) == abs(B[i] - B[i - 1])) {
            steps += abs(A[i] - A[i - 1]);
        } else if ((diff = abs(abs(A[i] - A[i - 1]) - abs(B[i] - B[i - 1]))) > 0) {
            if (abs(A[i] - A[i - 1]) > abs(B[i] - B[i - 1])) {
                steps += (diff + abs(B[i] - B[i - 1]));
            } else {
                steps += (diff + abs(A[i] - A[i - 1]));
            }
        }
    }
    return steps;
}
```

### Mine

```cpp
int Solution::coverPoints(vector<int> &A, vector<int> &B) {
    int n = A.size();
    if (n==0) return 0;
    int x = A[0], y = B[0], steps = 0;
    for (int i=0; i<n; i++) {
        int dx = abs(x - A[i]);
        int dy = abs(y - B[i]);
        steps += max(dx, dy);
        x = A[i];
        y = B[i];
    }
    return steps;
}
```

## Asked in

* Directi
