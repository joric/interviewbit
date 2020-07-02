# Repeat and Missing Number Array

https://www.interviewbit.com/problems/repeat-and-missing-number-array/

Please Note:
There are certain problems which are asked in the interview to also check how you take care of overflows in your problem.
This is one of those problems.
Please take extra care to make sure that you are type-casting your ints to long properly and at all places. Try to verify if your solution works if number of elements is as large as 105

 Food for thought :
Even though it might not be required in this problem, in some cases, you might be required to order the operations cleverly so that the numbers do not overflow. 
For example, if you need to calculate n! / k! where n! is factorial(n), one approach is to calculate factorial(n), factorial(k) and then divide them. 
Another approach is to only multiple numbers from k + 1 ... n to calculate the result. 
Obviously approach 1 is more susceptible to overflows.

You are given a read only array of n integers from 1 to n.

Each integer appears exactly once except A which appears twice and B which is missing.

Return A and B.

Note: Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

Note that in your output A should precede B.

## Example
```
Input:[3 1 2 5 3] 

Output:[3, 4] 

A = 3, B = 4
```

## SOLUTION 1
Sort the input array. Traverse the array and check for missing and repeating.
Time Complexity: O(n log n)

## SOLUTION 2
Use count array 
Time Complexity: O(n), O(n) space

## SOLUTION 3
Use elements as index and mark visited as negative (if input is positive)
Time Complexity: O(n)


## Solution

```cpp
vector < int >Solution::repeatedNumber (const vector < int >&A) {
    vector<int> arr = A; // remove const
    int x = 0, y = 0;
    
    for (int i = 0; i < arr.size(); i++) { 
        if (arr[abs(arr[i]) - 1] > 0) 
            arr[abs(arr[i]) - 1] = -arr[abs(arr[i]) - 1]; 
        else {
            x = abs(arr[i]);
        }
    }
    for (int i = 0; i < arr.size(); i++) { 
        if (arr[i] > 0) {
            y = i+1;
            break;
        }
    } 
    return {x, y};
}
```

## SOLUTION 4

Make two equations

1) S = n(n+1)/2 - x + y

2) P = 1*2*3* .. * n * y / x

Time Complexity: O(n)
This method can cause arithmetic overflow as we calculate product and sum of all array elements.


## SOLUTION 4.5 (editorial)
Sum(Actual) = Sum(1...N) + A - B
Sum(Actual) - Sum(1...N) = A - B

We need one more relation. How about the sum of squares?

Sum(1^2 .. N^2) and Sum(A[0]^2 .. A[N-1]^2)?

Sum(Actual Squares) = Sum(1^2 ... N^2) + A^2 - B^2
Sum(Actual Squares) - Sum(1^2 ... N^2) = (A - B)(A + B) = (Sum(Actual) - Sum(1...N))(A + B).

```cpp
vector<int> Solution::repeatedNumber(const vector<int> &A) {
    long sum = 0, squareSum = 0;
    for(int i=0; i<A.size(); i++) {
        sum += A[i];
        sum -= i+1;
        squareSum += long(A[i]) * A[i];
        squareSum -= long(i+1) * (i+1);
    }
    // sum = A - B, squareSum = A^2 - B^2 => squareSum / sum = A + B
    squareSum /= sum;
    int x = (sum + squareSum) / 2; // (A-B+A+B)/2 = A
    int y = squareSum - x;
    return {x, y};
}

/* faster version */

vector<int> Solution::repeatedNumber(const vector<int> &A) {
    long n=A.size(), sum = 0, square = 0;
    for(int i=0; i<n; i++) {
        sum += A[i];
        square += (long)A[i] * (long)A[i];
    }
    long diff= n*(n+1)/2 - sum;
    long a = n*(n+1)*(2*n+1)/6 - square;
    long b = a/diff;
    return {(b-diff)/2, (b+diff)/2};
}
```

## SOLUTION 5

Let x and y be the desired output elements.
Calculate XOR of all the array elements.
xor1 = arr[0]^arr[1]^arr[2] .. arr[n-1]

XOR the result with all numbers from 1 to n
xor1 = xor1^1^2^ .. ^n

In the result xor1, all elements would nullify each other except x and y.
All the bits that are set in xor1 will be set in either x or y.

So if we take any set bit (We have chosen the rightmost set bit in code)
of xor1 and make two sets of elements– one set of elements with same
bit set and other set with same bit not set.

By doing so, we will get x in one set and y in another set.
Now if we do XOR of all the elements in first set, we will get x,
and by doing same in other set we will get y.

```cpp
vector < int >Solution::repeatedNumber (const vector < int >&arr) {
    /* Will hold xor of all elements and numbers from 1 to n */
    int xor1;

    /* Will have only single set bit of xor1 */
    int set_bit_no;

    int i;
    int x = 0; // missing
    int y = 0; // repeated
    int n = arr.size();

    xor1 = arr[0];

    /* Get the xor of all array elements */
    for (i = 1; i < n; i++)
        xor1 = xor1 ^ arr[i];

    /* XOR the previous result with numbers from 1 to n */
    for (i = 1; i <= n; i++)
        xor1 = xor1 ^ i;

    /* Get the rightmost set bit in set_bit_no */
    set_bit_no = xor1 & ~(xor1 - 1);

    /* Now divide elements into two sets by comparing a rightmost set bit
       of xor1 with the bit at the same position in each element.
       Also, get XORs of two sets. The two XORs are the output elements.
       The following two for loops serve the purpose */

    for (i = 0; i < n; i++) {
        if (arr[i] & set_bit_no)
            /* arr[i] belongs to first set */
            x = x ^ arr[i];

        else
            /* arr[i] belongs to second set */
            y = y ^ arr[i];
    }

    for (i = 1; i <= n; i++) {
        if (i & set_bit_no)
            /* i belongs to first set */
            x = x ^ i;

        else
            /* i belongs to second set */
            y = y ^ i;
    }

    // NB! numbers can be swapped, maybe do a check ?
    int x_count = 0;
    for (int i=0; i<n; i++) {
        if (arr[i]==x)
            x_count++;
    }
    
    if (x_count==0)
        return {y, x};
    
    return {x, y};
}
```
## Asked in
* Amazon
