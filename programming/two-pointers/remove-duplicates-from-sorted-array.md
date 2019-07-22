# Remove Duplicates

https://www.interviewbit.com/problems/remove-duplicates


## Solution

```cpp
int Solution::removeDuplicates(vector<int> &A) {
    // [1,1,2]
    //  i   j
    // [1,2,2]
    //    i j
    
    if (A.size()<2)
        return A.size();
    int i=0;
    for (int j=1; j<A.size(); j++) {
        if (A[i]!=A[j]) {
            i++;
            A[i] = A[j];
        }
    }
    return i+1;
}

```