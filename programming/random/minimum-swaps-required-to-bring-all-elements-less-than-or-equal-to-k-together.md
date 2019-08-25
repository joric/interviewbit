# Minimum swaps required to bring all elements less than or equal to k together

https://www.interviewbit.com/problems/minimum-swaps-required-to-bring-all-elements-less-than-or-equal-to-k-together/

Given an array of integers A and an integer B, find and return the minimum number of swaps 
required to bring all the numbers less than or equal to B together.

### Note

It is possible to swap any two elements, not necessarily consecutive.

### Input Format
```
The first argument given is the integer array A.
The second argument given is the integer B.
```

### Output Format

Return the minimum number of swaps.

### Constraints
```
1 <= length of the array <= 100000
-10^9 <= A[i], B <= 10^9 
```

### For Example
```
Input 1:
    A = [1, 12, 10, 3, 14, 10, 5]
    B = 8
Output 1:
    2

Input 2:
    A = [5, 17, 100, 11]
    B = 20
Output 2:
    1
```

## Solution 
### Editorial
```cpp
int Solution::solve(vector<int> &A, int B){
    if(A.size()<=1) return 0;
    int cnt = 0, bad = 0;
    /// first find the count of numbers <= B
    /// now make window of size cnt and find the number of bad no(which are > B)
    /// now problem reduces to find the window containing min no of bad
    for(int i: A)
        cnt += (i<=B);
    
    int i=0,j=0,n=A.size(), len=INT_MAX;
    while(j<n){
        bad += (A[j]>B);
        j++;
        
        while(i<n && j-i>=cnt){
            len = min(len, bad);
            if(A[i]>B)
                bad -= 1;
            i++;
        }
    }
    return len==INT_MAX?0:len;
}
```
### Fastest
```cpp
int Solution::solve(vector<int> &A, int B) {
    /*int n=A.size();
    int cnt=0;
    for(int i=0;i<n;i++)
    {
        if(A[i]<=B)
            cnt++;
    }
    int val=0,ans;
    for(int i=0;i<cnt;i++)
    {
        if(A[i]>B)
            val++;
    }
    ans=val;
    for(int j=cnt;j<n;j++)
    {
        if(A[j]>B)
            val++;
        if(A[j-cnt]>B)
            val--;
        ans=min(ans,val);
    }
    return ans;*/
    int n=A.size();
    int cnt=0;
    for(int i=0;i<n;i++)
    {
        if(A[i]<=B)
            cnt++;
    }
    int val=0;
    for(int i=0;i<cnt;i++)
    {
        if(A[i]>B)
            val++;
    }
    int ans=val;
    int f=val;
    for(int j=cnt;j<n;j++)
    {
        if(A[j]>B)
            f++;
        if(A[j-cnt]>B)
            f--;
        ans=min(ans,f);
    }
    return ans;
}
```

### Lightweight
```cpp
int Solution::solve(vector<int> &arr, int k) {
    int n = arr.size();
    int count = 0; 
    for (int i = 0; i < n; ++i) 
        if (arr[i] <= k) 
            ++count;
    int bad = 0; 
    for (int i = 0; i < count; ++i) 
        if (arr[i] > k) 
            ++bad;
    int ans = bad; 
    for (int i = 0, j = count; j < n; ++i, ++j) { 
          
        // Decrement count of previous window 
        if (arr[i] > k) 
            --bad; 
          
        // Increment count of current window 
        if (arr[j] > k) 
            ++bad; 
          
        // Update ans if count of 'bad' 
        // is less in current window 
        ans = min(ans, bad); 
    } 
    return ans; 
}
```

### Mine
```cpp
int Solution::solve(vector<int> &arr, int k) {
    int n = arr.size();
    int count = 0; 
    for (int i = 0; i < n; ++i) 
        if (arr[i] <= k) 
            ++count; 
    int bad = 0; 
    for (int i = 0; i < count; ++i) 
        if (arr[i] > k) 
            ++bad; 
    int ans = bad; 
    for (int i = 0, j = count; j < n; ++i, ++j) { 
        if (arr[i] > k) 
            --bad; 
        if (arr[j] > k) 
            ++bad; 
        ans = min(ans, bad); 
    } 
    return ans; 
}
```
