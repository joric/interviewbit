# Find Duplicate in Array

https://www.interviewbit.com/problems/find-duplicate-in-array/

Given a read only array of n + 1 integers between 1 and n, find one number that repeats in linear time using less than O(n) space and traversing the stream sequentially O(1) times.

Sample Input: [3 4 1 4 1]

Sample Output: 1

If there are multiple possible answers ( like in the sample case above ), output any one.

If there is no duplicate, output -1

## Hint 1

Note that summing up the numbers or XOR of the numbers is not going to work in this case.

Can you try solving the problem in sqrt(n) space?

Split the numbers from 1 to n in sqrt(n) ranges so that range i corresponds to [sqrt(n) * i .. sqrt(n) * (i + 1)).

## Solution approach

Split the numbers from 1 to n in sqrt(n) ranges so that range i corresponds to [sqrt(n) * i .. sqrt(n) * (i + 1)).

Do one pass through the stream of numbers and figure out how many numbers fall in each of the ranges.

At least one of the ranges will contain more than sqrt(n) elements.

Do another pass and process just those elements in the oversubscribed range.

Using a hash table to keep frequencies, you'll find a repeated element.

This is O(sqrt(n)) memory and 2 sequential passes through the stream.

## Solution

### Editorial

```cpp
int Solution::repeatedNumber(const vector<int> &A) {
    int x = 0;
    int y = A[0];
    for (int i = 1; i < A.size(); i++) {
        x ^= i;
        y ^= A[i];
    }
    return x^y;
}
```

### Mine
```cpp
int Solution::repeatedNumber(const vector<int> &A) {
    int k=1;
    int i=2;
    int n=A.size();
    
    while(i<=n-1) {
        k=k^i;
        i+=1;
    }
    
    int temp = A[0];
    for(int j=1; j<n; j++)
        temp ^= A[j];
        
    return temp^k;
}
```

## Asked in

* Amazon
* VMWare
* Riverbed

