# Sliding Window Maxiumum

https://www.interviewbit.com/problems/sliding-window-maxiumum



The double-ended queue is the perfect data structure for this problem.
It supports insertion/deletion from the front and back.
The trick is to find a way such that the largest element in the window
would always appear in the front of the queue. How would you maintain
this requirement as you push and pop elements in and out of the queue?

You might notice that there are some redundant elements in the queue
that we shouldn't even consider about. For example, if the current
queue has the elements: [10 5 3], and a new element in the window has
the element 11. Now, we could have emptied the queue without considering
elements 10, 5, and 3, and insert only element 11 into the queue.

A natural way most people would think is to try to maintain the queue
size the same as the window's size. Try to break away from this thought
and try to think outside of the box. Removing redundant elements and storing
only elements that need to be considered in the queue is the key to achieve 
the efficient O(n) solution.

## Solution

```cpp

// editorial

vector<int> Solution::slidingMaximum(vector<int> &A, int w) {
    int n = A.size();
    vector<int> B;
    if (n < w) return B;
    B.resize(n - w + 1);
    deque<int> q;
    for (int i = 0; i < w; i++) {
        while (!q.empty() && A[i] >= A[q.back()])
            q.pop_back();
        q.push_back(i);
    }
    for (int i = w; i < n; i++) {
        B[i - w] = A[q.front()];
        while (!q.empty() && A[i] >= A[q.back()])
            q.pop_back();
        while (!q.empty() && q.front() <= i - w)
            q.pop_front();
        q.push_back(i);
    }
    B[n - w] = A[q.front()];
    return B;
}

```