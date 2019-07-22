# Min Xor Value

https://www.interviewbit.com/problems/min-xor-value


## Solution

```cpp
int Solution::findMinXor(vector<int> &arr) {
    int n = arr.size();
    sort(arr.begin(), arr.end()); 
    int minXor = INT_MAX; 
    int val = 0; 
    // calculate min xor of consecutive pairs 
    for (int i = 0; i < n - 1; i++)
        minXor = min(minXor, arr[i] ^ arr[i + 1]); 
        
    return minXor;
}
```