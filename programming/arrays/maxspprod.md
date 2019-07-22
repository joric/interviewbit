# MAXSPPROD

https://www.interviewbit.com/problems/maxspprod/

You are given an array A containing N integers. The special product of each ith integer in this array is defined as the product of the following:

**LeftSpecialValue**: For an index i, it is defined as the index j such that A[j]>A[i] (i>j). If multiple A[j]'s are present in multiple positions, the LeftSpecialValue is the maximum value of j. 

**RightSpecialValue**: For an index i, it is defined as the index j such that A[j]>A[i] (j>i). If multiple A[j]'s are present in multiple positions, the RightSpecialValue is the minimum value of j.

Write a program to find the maximum special product of any integer in the array.

**Input**: You will receive array of integers as argument to function.

**Return**: Maximum special product of any integer in the array modulo 1000000007.

**Note**: If j does not exist, the LeftSpecialValue and RightSpecialValue are considered to be 0.

**Constraints**: 1 <= N <= 10^5; 1 <= A[i] <= 10^9


## Solution

```cpp
int next_bigger(std::vector<int>& v, std::stack<int>& stack, int i){
    while(!stack.empty()){
        int j = stack.top();
        if (v[j] <= v[i]){
            stack.pop();
        }
        else{
            stack.push(i);
            return j;
        }
    }
    stack.push(i);
    return 0;
}

void right( std::vector<int>& v, std::vector<int>& r ){
    stack<int> stack;

    for(int i = v.size() - 1; i >= 0; --i){
        r[i] = next_bigger(v, stack, i);
        stack.push(i);
    }
}

int Solution::maxSpecialProduct(vector<int> &v) {
    vector<int> r(v.size());
    right(v, r);

    long mp = 0;
    std::stack<int> stack;
    for (int i = 0; i < v.size(); ++i)
    {
        long j = next_bigger(v, stack, i);
        long mp_i = j * r[i];
        if (mp < mp_i)
        {
            mp = mp_i;
        }
    }
    return mp % 1000000007;
}
```

