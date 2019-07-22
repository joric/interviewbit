# Rain Water Trapped

https://www.interviewbit.com/problems/rain-water-trapped

Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it is able to trap after raining.

Example :

Given [0,1,0,2,1,0,1,3,2,1,2,1], return 6.

![](http://i.imgur.com/0qkUFco.png)

The above elevation map is represented by array [0,1,0,2,1,0,1,3,2,1,2,1].

In this case, 6 units of rain water (blue section) are being trapped.

## Hint 1

Take a close look at any particular bin. How high can this hold water? If you can compute the answer of the above question for every bin you are done.

Every bin will store water which will depend on some prefix and suffix quantity. Can you figure it out now?

## Solution Approach

Instead of calculating area by height * width, we can think it in a cumulative way.
In other words, sum water amount of each bin(width=1).
Search from left to right and maintain a max height of left and right separately,
which is like a one-side wall of partial container.
Fix the higher one and flow water from the lower part.
For example, if current height of left is lower, we fill water in the left bin.
Until left meets right, we filled the whole container.

## Solution

### Editorial
```cpp
int Solution::trap(int A[], int n) {
    int left = 0;
    int right = n - 1;
    int res = 0;
    int maxleft = 0, maxright = 0;
    while (left <= right) {
        if (A[left] <= A[right]) {
            if (A[left] >= maxleft)
                maxleft = A[left];
            else
                res += maxleft - A[left];
            left++;
        } else {
            if (A[right] >= maxright)
                maxright = A[right];
            else
                res += maxright - A[right];
            right--;
        }
    }
    return res;
}

```

### Lightweight
```cpp
int Solution::trap(const vector<int> &A) {
    int result = 0;
    if (A.size() < 3) return 0;
    int i = 0, j = A.size() - 1;
    int maxLeft = A[i], maxRight = A[j];
    while (i < j) {
        if (A[i] < A[j]) {
            maxLeft = max(maxLeft, A[++i]);
            result += maxLeft - A[i];
        } else {
            maxRight = max(maxRight, A[--j]);
            result += maxRight - A[j];
        }
    }
    return result;
}
```

### Mine

```cpp
int Solution::trap(const vector<int> &v) {
    int i = 0, j = v.size()-1, iMax = 0, jMax = 0, res = 0;
    while (i < j) {
        iMax = max(iMax, v[i]), jMax = max(jMax, v[j]);
        res += iMax > jMax ? jMax - v[j--] : iMax - v[i++];
    }
    return res;
}
```