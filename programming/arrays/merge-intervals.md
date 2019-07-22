# Merge Intervals

https://www.interviewbit.com/problems/merge-intervals/

Given a set of non-overlapping intervals, insert a new interval into the intervals (merge if necessary).

You may assume that the intervals were initially sorted according to their start times.

Example 1:

Given intervals [1,3],[6,9] insert and merge [2,5] would result in [1,5],[6,9].

Example 2:

Given [1,2],[3,5],[6,7],[8,10],[12,16], insert and merge [4,9] would result in [1,2],[3,10],[12,16].

This is because the new interval [4,9] overlaps with [3,5],[6,7],[8,10].

Make sure the returned intervals are also sorted.

## Hint 1

This problem has a lot of corner cases which need to be handled correctly.

Let us first talk about the approach. 
Given all the intervals, you need to figure out the sequence of intervals which intersect with the given newInterval.

Lets see how we check if interval 1 (a,b) intersects with interval 2 (c,d):

Overlap case :
```
    a---------------------b                      OR       a------b
                c-------------------d                c------------------d
```
Non overlap case :
```
    a--------------------b   c------------------d
```
Note that if max(a,c) > min(b,d), then the intervals do not overlap. Otherwise, they overlap.

Once we figure out the intervals ( interval[i] to interval[j] ) which overlap with newInterval, note that we can replace all the overlapping intervals with one interval which would be

(min(interval[i].start, newInterval.start), max(interval[j].end, newInterval.end)).

Do make sure you cover the other corner cases.


## Solution Approach

Have you covered the following corner cases :

1) Size of interval array as 0.

2) newInterval being an interval preceding all intervals in the array.

    Given interval (3,6),(8,10), insert and merge (1,2)

3) newInterval being an interval succeeding all intervals in the array.

    Given interval (1,2), (3,6), insert and merge (8,10)

4) newInterval not overlapping with any interval and falling in between 2 intervals in the array.

    Given interval (1,2), (8,10) insert and merge (3,6) 

5) newInterval covering all given intervals.

    Given interval (3, 5), (7, 9) insert and merge (1, 10)


## Solution

```cpp
// A subroutine to check if intervals overlap or not.
bool doesOverlap(Interval a, Interval b) {
    return (min(a.end, b.end) >= max(a.start, b.start));
}

// Function to insert new interval and 
// merge overlapping intervals 

vector<Interval> Solution::insert(vector<Interval> &Intervals, Interval newInterval){ 
    vector<Interval> ans; 
    int n = Intervals.size(); 

    // If set is empty then simply insert
    // newInterval and return.
    if (n == 0) 
    { 
        ans.push_back(newInterval); 
        return ans; 
    }

    // Case 1 and Case 2 (new interval to be
    // inserted at corners) 
    if (newInterval.end < Intervals[0].start || 
            newInterval.start > Intervals[n - 1].end) 
    { 
        if (newInterval.end < Intervals[0].start) 
            ans.push_back(newInterval); 
  
        for (int i = 0; i < n; i++) 
            ans.push_back(Intervals[i]); 
  
        if (newInterval.start > Intervals[n - 1].end) 
            ans.push_back(newInterval); 
  
        return ans; 
    } 
  
    // Case 3 (New interval covers all existing) 
    if (newInterval.start <= Intervals[0].start && 
        newInterval.end >= Intervals[n - 1].end) 
    { 
        ans.push_back(newInterval); 
        return ans; 
    } 
  
    // Case 4 and Case 5 
    // These two cases need to check whether 
    // intervals overlap or not. For this we 
    // can use a subroutine that will perform 
    // this function. 
    bool overlap = true; 
    for (int i = 0; i < n; i++) 
    { 
        overlap = doesOverlap(Intervals[i], newInterval); 
        if (!overlap) 
        { 
            ans.push_back(Intervals[i]); 
  
            // Case 4 : To check if given interval 
            // lies between two intervals. 
            if (i < n && 
                newInterval.start > Intervals[i].end && 
                newInterval.end < Intervals[i + 1].start) 
                ans.push_back(newInterval); 
  
            continue; 
        } 
  
        // Case 5 : Merge Overlapping Intervals. 
        // Starting time of new merged interval is 
        // minimum of starting time of both 
        // overlapping intervals. 
        Interval temp; 
        temp.start = min(newInterval.start, 
                         Intervals[i].start); 
  
        // Traverse the set until intervals are 
        // overlapping 
        while (i < n && overlap) 
        { 
  
            // Ending time of new merged interval 
            // is maximum of ending time both 
            // overlapping intervals. 
            temp.end = max(newInterval.end, 
                           Intervals[i].end); 
            if (i == n - 1) 
                overlap = false; 
            else
                overlap = doesOverlap(Intervals[i + 1], 
                                          newInterval); 
            i++; 
        } 
  
        i--; 
        ans.push_back(temp); 
    } 
  
    return ans; 
} 
```
