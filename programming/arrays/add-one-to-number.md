# Add One To Number

https://www.interviewbit.com/problems/add-one-to-number

Given a non-negative number represented as an array of digits,
add 1 to the number (increment the number represented by the digits).

The digits are stored such that the most significant digit is at the head of the list.

### Example

If the vector has [1, 2, 3] the returned vector should be [1, 2, 4], as 123 + 1 = 124.

NOTE: Certain things are intentionally left unclear in this question which you should practice asking the interviewer.

For example, for this problem, following are some good questions to ask:

* Q: Can the input have 0's before the most significant digit. Or in other words, is 0 1 2 3 a valid input?
* A: For the purpose of this question, **YES**
* Q: Can the output have 0's before the most significant digit? Or in other words, is 0 1 2 4 a valid output?
* A: For the purpose of this question, **NO**. Even if the input has zeroes before the most significant digit.

## Hint 1

Note that because the order of covering the points is already defined, the problem just reduces to figuring out the way to calculate the distance between 2 points (A, B) and (C, D).

Can you think of a formula for calculating the distance in O(1) ?

## Solution Approach

Note that because the order of covering the points is already defined, the problem just reduces to figuring out the way to calculate the distance between 2 points (A, B) and (C, D).

Note that what only matters is X = abs(A-C) and Y = abs(B-D).

While X and Y are positive, you will move along the diagonal and X and Y would both reduce by 1. 
When one of them becomes 0, you would move so that in each step the remaining number reduces by 1.

In other words, the total number of steps would correspond to max(X, Y).

Take a few examples to convince yourself.

## Solution

```cpp
vector<int> Solution::plusOne(vector<int> &A) {
    int n = A.size();
    int carry = 1;
    vector<int> B;
    int ofs = 0;

    while(A[ofs]==0 && n-ofs>0)
        ofs++;

    for(int i=n-1; i>=ofs; i--) {
        int digit = A[i];
        if (carry > 0) {
            if (A[i]==9) {
                digit = 0;
                carry = 1;
            } else {
                digit = digit+1;
                carry = 0;
            }
        }
        B.push_back(digit);
    }

    if (carry)
        B.push_back(1);

    reverse(B.begin(), B.end());
    return B;
}
```

### Asked in

* Google
* Amazon

