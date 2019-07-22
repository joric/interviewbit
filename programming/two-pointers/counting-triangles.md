# Counting Triangles

https://www.interviewbit.com/problems/counting-triangles

You are given an array of N non-negative integers, A[0], A[1] ,..., A[N-1].

Considering each array element Ai as the edge length of some line segment,
count the number of triangles which you can form using these array values.

### Notes

You can use any value only once while forming each triangle. Order of choosing the edge lengths doesn't matter. Any triangle formed should have a positive area.
Return answer modulo 10^9 + 7. For example,
```
A = [1, 1, 1, 2, 2]
Return: 4
```
## Hint 1

We know that for side lengths A, B and C where A < B < C, they form a triangle if they follow the triangle inequality i.e.

A + B > C.

Suppose you are given a sorted array of side lengths in ascending order. How will you count the possible number of triangles?

## Solution Approach

First we sort the array of side lengths. So since

A[i] < A[j] < A[k] where i < j < k, therefore it is sufficient to check

A[i] + A[j] > A[k] to prove they form a triangle.

Thus for every i and j, we can find the maximum value of k such that the triangle inequality holds. 
Also we can also prove that for every such index i, we only have to increase the value of the k
(satisfying the above condition) for every iteration of j from i+1 to n.
Therefore, we get a O(n2) solution

## Solution

```cpp

int Solution::nTriang(vector<int> &A) {
    int N = A.size();
    if(N <= 2) return 0;
    sort(A.begin(), A.end());
    int ans = 0;
    for(int i = 0; i < N; i++) {    // first side
        int k = i + 2;
        for(int j = i + 1; j < N; j++) {    // second side
            for(; (k < N) && (A[i] + A[j] > A[k]); k++);
            ans = ans + (k - 1 - j);    // all indices of between j to k are possible
            if(ans >= 1000000007) ans = ans % 1000000007;
        }
    }
    return ans;
}

int Solution::nTriang(vector<int> &A) {
    
    int ans = 0, n = A.size(), num = pow(10, 9) + 7;
    
    if(!n) {
        return ans;
    }
    
    sort(A.begin(), A.end());
    
    for(int k = n - 1; k >= 0; k--){
        int i = 0, j = k - 1;
        while(i < j){
            long int miniSum = A[i] + A[j], maxi = A[k];
            if(miniSum > maxi){
                ans = (ans + (j - i)%num)%num;
                j--;
            }   
            else{
                i++;    
            }
        }
    }
    return ans;    
}

```
