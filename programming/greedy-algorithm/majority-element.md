# Majority Element

https://www.interviewbit.com/problems/majority-element




## Hint 1

Lets say you find 2 elements x and y which are different in the array.
If you removed those 2 elements, would the majority element change ?

## Hint 2

If we cancel out each occurrence of an element e with all the other elements
that are different from e then e will exist till end if it is a majority element.
Loop through each element and maintains a count of the element that has the potential
of being the majority element. If next element is same then increments the count,
otherwise decrements the count. If the count reaches 0 then update the potential
index to the current element and reset count to 1.


## Solution

```cpp

int Solution::majorityElement(const vector<int> &A) {
    int c = 0, x = 0;
    for (int i=0; i<A.size(); i++) {
        if (c==0) {
            x = A[i];
            c++;
        } else {
            if (x == A[i])
                c++;
            else
                c--;
        }
    }
    return x;
}

```