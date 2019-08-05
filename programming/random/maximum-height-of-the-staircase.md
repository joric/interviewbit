# Maximum height of the staircase

https://www.interviewbit.com/problems/maximum-height-of-the-staircase/?ref=random-problem


Given an integer A representing the square blocks. The height of each square block is 1. 

The task is to create a staircase of max height using these blocks.

The first stair would require only one block, the second stair would require two blocks and so on.

Find and return the maximum height of the staircase.

### Input Format

The only argument given is integer A.

### Output Format

Return the maximum height of the staircase using these blocks.

### Constraints

```
0 <= A <= 10^9
```

### For Example
```
Input 1:
    A = 10
Output 1:
    4

Input 2:
    A = 20
Output 2:
    5
```
## Solution

### Editorial
```cpp
int Solution::solve(int A) {
    int k = sqrt(2*A);
    if(k==0)
        return 0;
    if(A-k*(k+1)/2>=0)
        return k;
    else
        return k-1;
}
```
### Fastest
```cpp
int Solution::solve(int A) {
    long long l = 1, r = A, curr = 1;
    if(A == 0)
        return 0;
    while(l <= r)
    {
        long long mid = (l+r)/2;
        if((mid * (mid + 1))/2 <= A)
        {
            curr = mid;
            l = mid + 1;
        }
        else
            r = mid - 1;
    }
    return (int)curr;
}

```
### Lightweight
```cpp
int Solution::solve(int A) {
    int st = 0, en = 2*sqrt(A), m;
    int ans = 0;
    while (st <= en)
    {
        m = (st+en)/2;
        long long int num = ((m)*(m+1))/2;
        if (num < 0)    en = m-1;
        else if (num <= A)
        {
            ans = m;
            st = m+1;
        }
        else    en = m-1;
    }
    return ans;
}
```
### Mine
```cpp
int Solution::solve(int n) {
    int sum=0, i=1;
    while(sum < n) {
        sum += i;
        i++;
    }
    return sum != n ? i-2 : i-1;
}
```
