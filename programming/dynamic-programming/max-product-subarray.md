# Max Product Subarray

https://www.interviewbit.com/problems/max-product-subarray/

Find the contiguous subarray within an array (containing at least one number) which has the largest product.
Return an integer corresponding to the maximum product possible.

### Example
```
Input : [2, 3, -2, 4]
Return : 6 

Possible with [2, 3]
```

## Hint 1

This problem can be solved in different ways:

DP based solution: Try to compute the maximum positive/negative product ending at any index i.

Observation based solution: Maintain something similar to slider and try to figure out when to change the position of slider. Keep maintaing positive/negative maximum product too.

## Solution Approach

If there were no zeros or negative numbers, then the answer would definitely be the product of the whole array.

Now lets assume there were no negative numbers and just positive numbers and 0. In that case we could maintain a current maximum product which would be reset to A[i] when 0s were encountered. 

When the negative numbers are introduced, the situation changes ever so slightly. We need to now maintain the maximum product in positive and maximum product in negative. On encountering a negative number, the maximum product in negative can quickly come into picture.

## Solution

### Editorial
```cpp
int Solution::maxProduct(const vector<int> &A) {
    // store the result that is the max we have found so far
    int r = A[0], n = A.size();

    // imax/imin stores the max/min product of
    // subarray that ends with the current number A[i]
    for (int i = 1, imax = r, imin = r; i < n; i++) {
        // multiplied by a negative makes big number smaller, small number bigger
        // so we redefine the extremums by swapping them
        if (A[i] < 0)
            swap(imax, imin);

        // max/min product for the current number is either the current number itself
        // or the max/min by the previous number times the current one
        imax = max(A[i], imax * A[i]);
        imin = min(A[i], imin * A[i]);

        // the newly computed max value is a candidate for our global result
        r = max(r, imax);
    }
    return r;
}
```

### Fastest
```cpp
int Solution::maxProduct(const vector<int> &A) {
    int minm, maxm;
    minm=maxm=A[0];
    int ans=maxm;
    for(int i=1;i<A.size();i++){
        if(A[i]<0)swap(minm,maxm);
        minm=min(minm*A[i], A[i]);
        maxm=max(maxm*A[i], A[i]);
        ans=max(ans, max(minm, maxm));
    }
    return ans;
}
```

### Lightweight
```cpp
int Solution::maxProduct(const vector<int> &A) {
    int n = A.size();
    int maxi = 0, min_end = 1 , max_end = 1;
    for(int i = 0 ; i < n ; i++)
    {
        if(A[i] > 0)
        {
            max_end =( max_end == 0 ? A[i] : max_end*A[i]);
            min_end = ( min_end == 0 ? A[i] : min_end*A[i]);
        }
        else if(A[i] == 0)
        {
            max_end = 0;
            min_end = 0;
        }
        else
        {
            int t = max_end;
            max_end = min_end*A[i];
            min_end = (t == 0 ? A[i] : t*A[i]);
        }
        maxi = max(maxi , max_end);
    }
    return maxi;
}
```

## Asked in

* Amazon
* LinkedIn
* Microsoft

