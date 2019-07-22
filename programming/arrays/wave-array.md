# Wave Array

https://www.interviewbit.com/problems/wave-array/

Given an array of integers, sort the array into a wave like array and return it, 
In other words, arrange the elements into a sequence such that a1 >= a2 <= a3 >= a4 <= a5 ...

## Example

Given [1, 2, 3, 4]

One possible answer : [2, 1, 4, 3]

Another possible answer : [4, 1, 3, 2]

NOTE : If there are multiple answers possible, return the one thats lexicographically smallest. 

So, in example case, you will return [2, 1, 4, 3] 

## Hint 1

Hint 1 : Sorting. 
Would it help if the array is sorted in ascending order ?

## Solution Approach

array = {5, 1, 3, 4, 2}

Sort the above array. 

array = {1, 2, 3, 4, 5}

Now swap adjacent elemets in pairs.

swap(1, 2)
swap(3, 4)

Now, our array = {2, 1, 4, 3, 5}

and voila!, the array is in the wave form.


## Solution


### Sort and swap adjacent pairs

```cpp
vector<int> Solution::wave(vector<int> &A) {
    std::sort(A.begin(),A.end());
    int n = A.size();
    for(int i=0;i<n-1;i+=2) swap(A[i], A[i+1]);
    return A;
}

```

### Use Median

```cpp
// find median, everything to the left is even, to the right is odd
vector<int> Solution::wave(vector<int> &A) {
    // O(n) solution
    int n = A.size();
    nth_element(A.begin(), A.begin() + n/2, A.end()); // O(n)
    int median = A[n/2];
    // everything greater than median goes even, odd otherwise
    int even = 0;
    int odd = 1;
    vector<int> B(n);
    for(int i = 0; i < n ; i++) {
        if(A[i]<median) {
            B[odd] = A[i];
            odd = odd + 2;
        } else {
            B[even] = A[i];
            even = even + 2;  
        }
    }
    return B;
}
```
