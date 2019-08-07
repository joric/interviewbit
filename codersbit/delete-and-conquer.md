# Delete and Conquer

https://www.interviewbit.com/problems/delete-and-conquer/

Given an array A of N integers, you are allowed to create a new array B by deleting some (possibly none) elements from array A.

What are the minimum number of elements you have to delete from the A to get B such that every element in B is divisble by every other element in B.

### Constraints

```
1.   1 <= N <= 100000
2.   1 <= A[i] <= 1000000000
```

### Input

A single array A.

### Output

Minimum number of elements to delete from A such that every element in the new array
is divisble by every other element in the new array

### Example

```
Input:
A : 10 20 10 20 10 5

Output:
3
```

### Explanation

The optimal choice is to delete the two 20's and the single 5 so as to get B as :10 10 10.

## Hint 1

Try to find the pattern.

## Solution Approach

You have to find the maximum repeating number in A, as only the same numbers are divisible by each other.

## Solution

### Editorial
```cpp
int Solution::deleteandconquer(vector<int> &A) {
    int n = A.size();
    unordered_map<int,int> mp;
    mp.clear();
    for (auto it : A)mp[it]++;
    int ans = n;
    for (auto it : mp) {
        ans = min(ans, n-it.second);
    }
    return ans;
}
```
### Fastest
```cpp
int Solution::deleteandconquer(vector<int> &A) {
    
    vector<int> freq(*max_element(A.begin(),A.end())+1,0);
    long long max=-1;
    for(int i=0;i<A.size();i++)
    {
        freq[A[i]]++;
        if(max<freq[A[i]]) max=freq[A[i]];
    }
    
    return (A.size()-max);
}
```
### Lightweight
```cpp
int Solution::deleteandconquer(vector<int> &A) {
    if(A.size()==1)return 0;
    sort(A.begin(),A.end());
    int ans=0,c=1,cur=A[0];
    for(int i=1;i<A.size();i++){
        if(A[i]==A[i-1]){
            c++;
        }
        else{
            ans=max(c,ans);
            c=1;
        }
    }
    ans=max(ans,c);
    return A.size()-ans;
}
```

### Mine
```cpp
int Solution::deleteandconquer(vector<int> &a) {
    unordered_map<int, int> h;
    int n = 0;
    for (int x:a) n = max(n, ++h[x]);
    return a.size() - n;
}
```

### Another Mine
```cpp
int Solution::deleteandconquer(vector<int> &a) {
    sort(a.begin(), a.end());
    int maxc = 0, c = 1;
    for (int i=1; i<a.size(); i++)
        if (a[i-1]==a[i]) c++; else maxc = max(maxc, c), c = 1;
    return a.size() - max(maxc, c);
}
```
