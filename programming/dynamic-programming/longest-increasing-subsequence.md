# Longest Increasing Subsequence

https://www.interviewbit.com/problems/longest-increasing-subsequence/

Find the longest increasing subsequence of a given sequence / array.

In other words, find a subsequence of array in which the subsequence’s elements are in strictly increasing order, and in which the subsequence is as long as possible. 
This subsequence is not necessarily contiguous, or unique.
In this case, we only care about the length of the longest increasing subsequence.

Example :

```
Input : [0, 8, 4, 12, 2, 10, 6, 14, 1, 9, 5, 13, 3, 11, 7, 15]
Output : 6
```

The sequence: 
```
[0, 2, 6, 9, 13, 15] or [0, 4, 6, 9, 11, 15] or [0, 4, 6, 9, 13, 15]
```

## Hint 1

Try to compute longest increasing subsequence ending at ith position for all i.

Think how can you use answers ending on 1st,2nd,3rd,….(i-1)th positions for computing answers ending on ith position.

Hint: Expected Complexity - O(n^2)

## Solution Approach

```
There is a straight-forward Dynamic Programming solution in O(n^{2}) time.
Though this is asymptotically equivalent to the Longest Common Subsequence version of the solution,
the constant is lower, as there is less overhead.

Let A be our sequence a_{1},a_{2},\ldots ,a_{n}. Define q_{k}
as the length of the longest increasing subsequence of A,
subject to the constraint that the subsequence must end on the element a_{k}.
The longest increasing subsequence of A must end on some element of A,
so that we can find its length by searching for the maximum value of q.
All that remains is to find out the values q_{k}.

But q_{k} can be found recursively, as follows:
consider the set S_{k} of all i<k such that a_{i}<a_{k}.
If this set is null, then all of the elements that come before a_{k} are greater than it,
which forces q_{k}=1. Otherwise, if S_{k} is not null,
then q has some distribution over S_{k}. By the general contract of q,
if we maximize q over S_{k}, we get the length of the longest increasing subsequence in S_{k};
we can append a_{k} to this sequence, to get that:

q_{k}=max(q_{j}|j\in S_{k})+1
If the actual subsequence is desired,
it can be found in O(n) further steps by moving backward through the q-array,
or else by implementing the q-array as a set of stacks,
so that the above "+ 1" is accomplished by "pushing" a_{k} into a copy of the maximum-length stack seen so far.
```

## Solution
### Editorial
```
int Solution::lis(const vector<int> &V) {
    if (V.size() == 0) return 0;
    int longest[V.size() + 1];
    int maxLen = 1;
    memset(longest, 0, sizeof(longest));
    // longest[i] denotes the maximum length of increasing subsequence that ends
    // with V[i].
    longest[0] = 1;
    for (int i = 1; i < V.size(); i++) {
        longest[i] = 1;
        // V[i] can only come after any V[j] such that V[j] < V[i].
        // We try appending V[i] after every such subsequence and update our
        // longest[i].
        for (int j = 0; j < i; j++) {
            if (V[j] < V[i]) longest[i] = max(longest[i], longest[j] + 1);
        }
        maxLen = max(maxLen, longest[i]);
    }
    return maxLen;
}

```

### Mine
```cpp
int Solution::lis(const vector<int> &A) {
    if(A.size() == 0){
        return 0;
    }
    vector<int> val(A.size(), 1);
    int sol = 1;
    for(int i = 1; i < A.size(); i++){
        for(int j = 0; j < i; j++){
            if(A[i] > A[j]){
                val[i] = max(val[j]+1, val[i]);
                if(val[i] > sol){
                    sol = val[i];
                }
            }
        }
    }
    return sol;
}
```
## Asked in

* Facebook
* Yahoo
* Epic systems
* Amazon
* Microsoft

