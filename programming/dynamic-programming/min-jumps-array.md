# Min Jumps Array

https://www.interviewbit.com/problems/min-jumps-array/


Given an array of non-negative integers, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Your goal is to reach the last index in the minimum number of jumps.

Example :

Given array A = [2,3,1,1,4]

The minimum number of jumps to reach the last index is 2. (Jump 1 step from index 0 to 1, then 3 steps to the last index.)

If it is not possible to reach the end index, return -1.

## Hint 1
Dp solution is quite straight-forward here, but greedy will also work here.

Can you come up with either of the solution now?

## Solution Approach

Greedy will work here. Think why.

With the current number of steps, try to maintain the maximum index which is reachable. When you exceed the index, you have to increase the number of steps, and at that instance you can increase the maximum index reachable accordingly.

## Solution

### Editorial
```cpp
int Solution::jump(int A[], int n) {
    if(n <= 1){
        return 0;
    }
    int maxReachPos = A[0];
    int curMaxReachPos = A[0];
    int curStep = 1;
    for(int i = 1; i <= maxReachPos; i++){
        if(i == n - 1){
            return curStep;
        }
        curMaxReachPos = max(curMaxReachPos, i + A[i]);
        if(i == maxReachPos){
            if (curMaxReachPos <= i) return -1;
            maxReachPos = curMaxReachPos;
            curStep++;
        }
    }
    return -1;
}
```

### Mine

```cpp
int Solution::jump(vector<int> &A) {
    int n=A.size();
    if(n==1) return 0;
    int max_reach=A[0],step=A[0],jump=1;
    for(int i=1;i<n-1;i++){
        max_reach=max(max_reach,i+A[i]);
        step--;
        if(step<0) return -1;
        if(step==0){
         jump++;
         step=max_reach-i; 
        }
    }
    if(max_reach>=n-1) return jump;
    return -1;
}
```

## Asked in
* Amazon
* Ebay
* Google


