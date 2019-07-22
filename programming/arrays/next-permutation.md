# Next Permutation

https://www.interviewbit.com/problems/next-permutation/

Implement the next permutation, which rearranges numbers into the numerically next greater permutation of numbers.

If such arrangement is not possible, it must be rearranged as the lowest possible order ie, sorted in an ascending order.

The replacement must be in-place, do not allocate extra memory.

Examples:

1,2,3 -> 1,3,2

3,2,1 -> 1,2,3

1,1,5 -> 1,5,1

20, 50, 113 -> 20, 113, 50

Inputs are in the left-hand column and its corresponding outputs are in the right-hand column.

Warning : DO NOT USE LIBRARY FUNCTION FOR NEXT PERMUTATION. Use of Library functions will disqualify your submission retroactively and will give you penalty points.

## Hint 1

You can try out few test cases to see what the pattern is or what exactly is the flow of numbers from initial sequence to final sequence.

## Solution Approach

It might help to write down the next permutation on paper to see how and when the sequence changes.

You'll realize the following pattern :

The suffix which gets affected is in a descending order before the change.

A swap with the smaller element happens and then we reverse the affected suffix.

1 2 3 -> 1 3 2   // Suffix being just the 3.

1 2 3 6 5 4  -> 1 2 4 3 5 6 // Suffix being 6 5 4 in this case.

## Solution

### Editorial

```cpp
void Solution::nextPermutation(vector<int> &num) {
    int len = num.size();
    int i, j;
    for (i = len - 2; i >= 0; i--)
        if (num[i] < num[i + 1])
            break;

    if (i == -1) {
        reverse(num.begin(), num.end());
        return;
    }

    for (j = len - 1; j > i; j--)
        if (num[j] > num[i])
            break;

    swap(num[i], num[j]);
    reverse(num.begin() + i + 1, num.end());
    return;
}
```

## Asked in

* Microsoft
* Amazon

