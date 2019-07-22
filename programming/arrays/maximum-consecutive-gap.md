# Maximum Consecutive Gap

https://www.interviewbit.com/problems/maximum-consecutive-gap/


Given an unsorted array, find the maximum difference between the successive elements in its sorted form.

Try to solve it in linear time/space.

Example :
```
Input : [1, 10, 5]
Output : 5 
```

Return 0 if the array contains less than 2 elements.

You may assume that all the elements in the array are non-negative integers and fit in the 32-bit signed integer range.
You may also assume that the difference will not overflow.


## Hint 1

Any form of sorting is going to be at least O(n * log n), so we need to think outside of sorting.

Also, you can use extra O(n) space.

Try to think starting from maximum and minimum of array.

How can you use the gap between them to separate elements into different blocks/buckets in such a way that you dont have to evaluate the answer for elements within buckets

## Solution Approach

Now, first try to think if you were already given the minimum value MIN and maximum value MAX in the array of size N, under what circumstances would the max gap be minimum and maximum ?

Obviously, maximum gap will be maximum when all elements are either MIN or MAX making maxgap = MAX - MIN.

Maximum gap will be minimum when all the elements are equally spaced apart between MIN and MAX. Lets say the spacing between them is gap.

So, they are arranged as

MIN, MIN + gap, MIN + 2*gap, MIN + 3*gap, ... MIN + (N-1)*gap 
where

MIN + (N-1) * gap = MAX 
=> gap = (MAX - MIN) / (N - 1). 
So, we know now that our answer will lie in the range [gap, MAX - MIN].

Now, if we know the answer is more than gap, what we do is create buckets of size gap for ranges

`[MIN, MIN + gap), [Min + gap, `MIN` + 2 * gap)` ... and so on

There will only be (N-1) such buckets. We place the numbers in these buckets based on their value.

If you pick any 2 numbers from a single bucket, their difference will be less than gap, and hence they would never contribute to maxgap ( Remember maxgap >= gap ). We only need to store the largest number and the smallest number in each bucket, and we only look at the numbers across bucket.

Now, we just need to go through the bucket sequentially ( they are already sorted by value ), and get the difference of min_value with max_value of previous bucket with at least one value. We take maximum of all such values.


## Solution

### Lightweight
```cpp

int Solution::maximumGap(const vector<int> &A) {

    if (A.size() == 1)
        return 0;

    int mn = A[0];
    int mx = A[0];

    for (int i = 0; i < A.size(); i++) {
        mn = min(mn, A[i]);
        mx = max(mx, A[i]);
    }

    int gap = sqrt(mx - mn + 1);
    int k = (mx - mn + 1) / gap;
    if ((mx - mn + 1) % gap)
        k++;

    int mini[k], maxi[k], flag[k];

    for (int i = 0; i < k; i++) {
        mini[i] = INT_MAX;
        maxi[i] = INT_MIN;
        flag[i] = 0;
    }

    for (int i = 0; i < A.size(); i++) {
        mini[(A[i] - mn) / gap] = min(A[i], mini[(A[i] - mn) / gap]);
        maxi[(A[i] - mn) / gap] = max(A[i], maxi[(A[i] - mn) / gap]);
        flag[(A[i] - mn) / gap] = 1;
    }

    int cur = -1, ans = 0;

    for (int i = 0; i < k; i++) {
        if (flag[i] && cur >= 0)
            ans = max(ans, mini[i] - maxi[cur]);
        if (flag[i])
            cur = i;
    }
    return ans;
}
```

### Mine
```cpp
int Solution::maximumGap(const vector<int> &A) {
    // Do not write main() function.
    // Do not read input, instead use the arguments to the function.
    // Do not print the output, instead return values as specified
    // Still have a doubt. Checkout www.interviewbit.com/pages/sample_codes/ for more details

    if (A.size() < 2) {
        return 0;
    }

    vector<int> forMin(A.size(), -1);
    vector<int> forMax(A.size(), -1);

    int max_dist = 0, mini = INT_MAX, maxi = INT_MIN;
    int gap = 0, bucket = 0, ind = 0;

    for (int i = 0; i < A.size(); i++) {
        if (A[i] < mini) {
            mini = A[i];
        }
        if (A[i] > maxi) {
            maxi = A[i];
        }
    }

    gap = maxi - mini;
    gap = gap / (A.size() - 1);

    if (gap == 0) {
        return maxi - mini;
    }

    for (int i = 0; i < A.size(); i++) {
        bucket = (int)((A[i] - mini) / gap);
        if (forMin[bucket] < 0) {
            forMin[bucket] = A[i];
            forMax[bucket] = A[i];
        } else {
            forMin[bucket] = min(A[i], forMin[bucket]);
            forMax[bucket] = max(A[i], forMax[bucket]);
        }
    }

    int max_diff = 0;

    for (int i = 0; i < forMin.size(); i++) {
        if (forMin[i] >= 0) {
            max_diff = max(max_diff, forMin[i] - forMax[ind]);
            ind = i;
        }
    }

    return max_diff;
}
```
## Asked in
* Hunan Asset
