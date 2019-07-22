# Merge Overlapping Intervals

https://www.interviewbit.com/problems/merge-overlapping-intervals/

Given a collection of intervals, merge all overlapping intervals.

For example:

Given [1,3],[2,6],[8,10],[15,18],

return [1,6],[8,10],[15,18].

Make sure the returned intervals are sorted.

## Hint 1

First try to figure out cases for two intervals to be merged.

Also, what can you do in order to simulate the process easily?

## Solution Approach


Given all the intervals, you need to figure out the sequence of intervals which intersect.

Lets see how we check if interval 1 (a,b) intersects with interval 2 (c,d):

Overlap case :

```
a---------------------b          OR     a------b
            c-------------------d           c------------------d
```

Non overlap case :

```
a--------------------b   c------------------d
```

If max(a,c) > min(b,d), then the intervals do not overlap. Otherwise, they overlap.

NOTE: Do you think your life will be easier if you could sort the intervals based on the starting point?


## Solution

```cpp
vector<Interval> Solution::merge(vector<Interval> &arr) {
    
    // Sort Intervals in decreasing order of start time 
    int n = arr.size();
    sort(arr.begin(), arr.end(), [](const Interval &a, const Interval &b) {return a.start > b.start;}); 
  
    int index = 0; // Stores index of last element 
    // in output array (modified arr[]) 
  
    // Traverse all input Intervals 
    for (int i=0; i<n; i++) { 
        // If this is not first Interval and overlaps 
        // with the previous one 
        if (index != 0 && arr[index-1].start <= arr[i].end) { 
            while (index != 0 && arr[index-1].start <= arr[i].end) { 
                // Merge previous and current Intervals 
                arr[index-1].end = max(arr[index-1].end, arr[i].end); 
                arr[index-1].start = min(arr[index-1].start, arr[i].start); 
                index--; 
            } 
        } 
        else // Doesn't overlap with previous, add to 
            // solution 
            arr[index] = arr[i];
        index++; 
    }
    arr.resize(index);
    
    sort(arr.begin(), arr.end(), [](const Interval &a, const Interval &b) {return a.start < b.start;}); 
    return arr;
} 
```

## Asked in

* Google
* Amazon

