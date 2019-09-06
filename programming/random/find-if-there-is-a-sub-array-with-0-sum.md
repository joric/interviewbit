# Find if there is a sub-array with 0 sum

https://www.interviewbit.com/problems/find-if-there-is-a-sub-array-with-0-sum/


Given an array of integers A, find and return whether the given array contains a subarray with a sum equal to 0.

If the given array contains a sub-array with sum zero return 1 else return 0.

Note: Length of sub array should be at least one.

### Input Format

The only argument given is the integer array A.

### Output Format

Return whether the given array contains a subarray with a sum equal to **0**.

### Constraints
```
1 <= length of the array <= 100000
-10^9 <= A[i] <= 10^9 
```
### For Example
```
Input 1:
    A = [1, 2, 3, 4, 5]
Output 1:
    0

Input 2:
    A = [5, 17, -22, 11]
Output 2:
    1
```

## Hint 1

Try using prefix sum array.

## Solution Approach

The idea is to iterate through the array and for every element A[i],
calculate sum of elements form 0 to i (this can simply be done as sum += arr[i]).

If the current sum has been seen before, then there is a zero sum array.

Hashing is used to store the sum values, so that we can quickly store sum and
find out whether the current sum is seen before or not.

## Solution
### Editorial
```cpp
int Solution::solve(vector<int> &arr) {
    unordered_set<long long int> st;
    long long int sum=0;
    for(int i=0;i<arr.size();i++)
    {
        sum+=arr[i];
        if(sum==0||st.find(sum)!=st.end())
        {
            return 1;
        }
        st.insert(sum);
    }
    return 0;
}
```
### Fastest
```cpp
int Solution::solve(vector<int> &A) {
    int n=A.size();
    
    vector<long long> sum(n,0);
    sum[0]=A[0];
    for(int i=1;i<n;i++){
        sum[i]=sum[i-1]+A[i];
    }
    sort(sum.begin(),sum.end());
    
    for(int i=0;i<n-1;i++){
        if(sum[i]==0 || sum[i]==sum[i+1]){
            return true;
        }
    }
    return false;
}
```

### Lightweight
```cpp
int Solution::solve(vector<int> &A) {
    int n=A.size();
    
    vector<long long> sum(n,0);
    sum[0]=A[0];
    for(int i=1;i<n;i++){
        sum[i]=sum[i-1]+A[i];
    }
    sort(sum.begin(),sum.end());
    
    for(int i=0;i<n-1;i++){
        if(sum[i]==0 || sum[i]==sum[i+1]){
            return true;
        }
    }
    return false;
}
```
