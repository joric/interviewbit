# First Missing Integer

https://www.interviewbit.com/problems/first-missing-integer

Given an unsorted integer array, find the first missing positive integer.

Example:

Given [1,2,0] return 3,

[3,4,-1,1] return 2,

[-8, -7, -6] returns 1

Your algorithm should run in O(n) time and use constant space.

## Hint 1

To simply solve this problem, search all positive integers, starting from 1 in the given array. We may have to search at most n+1 numbers in the given array. So this solution takes O(n^2) in worst case.

We can use sorting to solve it in lesser time complexity. We can sort the array in O(nLogn) time.

Once the array is sorted, then a linear scan of the array is all that remains to be done.

So this approach takes O(nLogn + n) time which is O(nLogn).

We can also use hashing. We can build a hash table of all positive elements in the given array.

Once the hash table is built, all positive integers, starting from 1 can be searched here. As soon as we find a number which is not there in the hash table, we return it.

Approximately, this approach may take O(n) time, but it requires O(n) extra space.

But what we are really looking for is a O(n) time, O(1) space solution.

Note that ( N being the size of the array ), A[i]<=0 and A[i]>N are not the numbers you are looking for because the missing positive integer will be in the range [1, N+1].

The answer will be N+1 only if all of the elements of the array are exact one occurrence of [1, N].

If using extra space was an option, creating buckets would have been an easy solution.

Creating an array of size N initialized to 0, for every value A[i] which lies in the range [1, N], we would have incremented its count in the array. Consequently, we would traverse the array to find the first array position with value 0, hence finding our answer.

However, since we are not allowed buckets, can we use the existing array as bucket somehow?

## Solution Approach

Note: numbers A[i]<=0 and A[i]>N ( N being the size of the array ) is not important
to us since the missing positive integer will be in the range [1, N+1].

The answer will be N+1 only if all of the elements of the array are exact one occurrence of [1, N].

Creating buckets would have been an easy solution if using extra space was allowed.

An array of size N initialized to 0 would have been created.

For every value A[i] which lies in the range [1, N], its count in the array would have been incremented.

Finally, traversing the array would help to find the first array position with value 0 and that will be our answer. 

However, usage of buckets is not allowed, can we use the existing array as bucket somehow?

Now, since additional space is not allowed either, the given array itself needs to be used to track it.

It may be helpful to use the fact that the size of the set we are looking to track is [1, N]

which happens to be the same size as the size of the array.

This means we can use the array to track these elements.

We traverse the array and if A[i] is in [1,N] range, we try to put in the index of same value in the array.

## Solution

```cpp
/* editorial */

int Solution::firstMissingPositive (vector < int >&A) {
    int n = A.size ();

    for (int i = 0; i < n; i++) {
        if (A[i] > 0 && A[i] <= n) {
            int pos = A[i] - 1;
            if (A[pos] != A[i]) {
                swap (A[pos], A[i]);
                i--;
            }
        }
    }
    for (int i = 0; i < n; i++) {
        if (A[i] != i + 1)
            return (i + 1);
    }
    return n + 1;
}


/* something working */

/*
  we shift all negative to the left to get rid of them
  then mark all positive as visited using sign
  and return first index value which is positive
*/

/* Utility function that puts all  
non-positive (0 and negative) numbers on left  
side of arr[] and return count of such numbers */
int segregate (vector<int> &arr)  {  
    int j = 0, i;  
    for(i = 0; i < arr.size(); i++) {  
        if (arr[i] <= 0){  
            swap(arr[i], arr[j]);  
            j++; // increment count of non-positive integers  
        }  
    }  
    return j;
}  
  
/* Find the smallest positive missing number  
in an array that contains all positive integers */
int findMissingPositive(vector<int> &arr, int size)  
{  
    int i;  
    // Mark arr[i] as visited by making arr[arr[i] - 1] negative.  
    // Note that 1 is subtracted because index start  
    // from 0 and positive numbers start from 1  
    for(i = 0; i < size; i++)  
    {  
        if(abs(arr[i]) - 1 < size && arr[ abs(arr[i]) - 1] > 0)  
        arr[ abs(arr[i]) - 1] = -arr[ abs(arr[i]) - 1];  
    }  
      
    // Return the first index value at which is positive  
    for(i = 0; i < size; i++)  
        if (arr[i] > 0)  
            // 1 is added because indexes start from 0  
            return i+1;  
      
    return size+1;  
}  

int Solution::firstMissingPositive(vector<int> &arr) {
    // First separate positive and negative numbers  
    int shift = segregate(arr);
    // Shift the array and call findMissingPositive for  
    // positive part
    
    rotate(arr.begin(), arr.begin()+shift, arr.end());
    
    return findMissingPositive(arr, arr.size() - shift);
}
```
## Asked in

* Model N
* InMobi
* Amazon


