# N/3 Repeat Number

https://www.interviewbit.com/problems/n3-repeat-number/

## Hint 1

It works to simply pick all elements one by one. For every picked element, count its occurrences by traversing the array. 
If count becomes more than n/3, then print the element. Time Complexity of this method would be O(n^2).

A better solution is to use sorting.

First, sort all elements using a O(nLogn) algorithm. All required elements in a linear scan of array can be found once the array is sorted.

So overall, time complexity of this method is O(nLogn) + O(n) which is O(nLogn).

However, a linear solution is needed here.

Note: if at any instance, you have three distinct elements from the array, if you remove them from the array, your answer does not change.

Try to base your solution idea on the above fact.

Would it help to maintain two elements from array with their count as you traversed the array ?

## Solution Approach

It is important to note that if at a given time, you have 3 distinct element from the array, if you remove them from the array, your answer does not change.

Assume that we maintain 2 elements with their counts as we traverse along the array.

When we encounter a new element, there are 3 cases possible :

We don't have 2 elements yet. So add this to the list with count as 1.

This element is different from the existing 2 elements. As we said before, we have 3 distinct numbers now. Removing them does not change the answer. So decrement 1 from count of 2 existing elements. If their count falls to 0, obviously its not a part of 2 elements anymore.

The new element is same as one of the 2 elements. Increment the count of that element.

Consequently, the answer will be one of the 2 elements left behind. If they are not the answer, then there is no element with count > N / 3.

## Solution

### Misra Gries (works)

```cpp
int Solution::repeatedNumber(const vector<int> &a) {
    // misra gries aka boyer-moore with extra steps
    int c1 = 0, c2 = 0, x1 = -1, x2 = -1;
    for (auto x:a)
        if (x1 == x) c1++;
        else if (x2 == x) c2++;
        else if (c1 == 0) c1++, x1 = x;
        else if (c2 == 0) c2++, x2 = x;
        else c1--, c2--;
        
    c1 = 0, c2 = 0;
    for (auto x:a)
        if (x==x1) c1++;
        else if (x==x2) c2++;
        
    int n = a.size();
    return c1>n/3 ? x1 : c2>n/3 ? x2 : -1;
}
```

### Boyer-Moore (does not work)

```cpp
int Solution::repeatedNumber(const vector<int> &a) {
    int x = -1, c = 0; for (auto z:a) if (c == 0) x = z, c++; else c += x == z ? 1 : -1;
    c = 0; for (auto z:a) if (x==z) c++;
    return c > a.size()/3 ? x : -1;
}

```
