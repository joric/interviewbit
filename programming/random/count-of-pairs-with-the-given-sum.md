# Count of pairs with the given sum

https://www.interviewbit.com/problems/count-of-pairs-with-the-given-sum


Given a sorted array of distinct integers A and an integer B, 
find and return how many pair of integers ( A[i], A[j] ) such that i != j have sum equal to B.
```
Input Format

The first argument given is the integer array A.
The second argument given is integer B.
Output Format

Return the number of pairs for which sum is equal to B.
Constraints

1 <= length of the array <= 100000
1 <= A[i] <= 10^9 
1 <= B <= 10^9
For Example

Input 1:
    A = [1, 2, 3, 4, 5]
    B = 5
Output 1:
    2

Input 2:
    A = [5, 10, 20, 100, 105]
    B = 110
Output 2:
    2
```

## Solution

### Editorial
```cpp
int Solution::solve(vector<int> &A, int B) {
     int n = A.size();
    int i=0, j = n-1;
    int count = 0;
    while(i < j) {
        if(A[j] + A[i] == B) { count ++; i++; j--;}
        else if(A[j] + A[i] > B)  j--;
        else if (A[j] +A[i] < B) i++;
    }
    return count;
}
```
### Fastest
```cpp
int Solution::solve(vector<int> &A, int B) {
    int i = 0;
    int j = A.size() - 1;
    int ct = 0;
    while (i < j) {
        if (A[i] == B - A[j]) {
            ct++;
            i++;
            j--;
        } else if (A[i] < B - A[j]) {
            i++;
        } else {
            j--;
        }
    }
    return ct;
}

```

### Lightweight
```cpp
int Solution::solve(vector<int> &v, int k) {
    int i=0,j=v.size()-1;
    int cnt=0;
    while(i<j){
        if((v[i]+v[j])==k)
            cnt++;
        if(v[i]+v[j]<=k)
            i++;
        else j--;
    }
    return cnt;
}
```

### Python (passes tests)
```python
class Solution:
    def solve(self, arr, sum):
        m = {}
        for x in arr:
            m[x] = m.get(x,0)+1
        count = 0
        for x in arr:
            count += m.get(sum - x,0)
            if sum-x == x:
                count -= 1
        return count/2
```
