# Painters Partition Problem

https://www.interviewbit.com/problems/painters-partition-problem


You have to paint N boards of length {A0, A1, A2, A3 ... AN-1}. There are K painters available and you are also given how much time a painter takes to paint 1 unit of board. You have to get this job done as soon as possible under the constraints that any painter will only paint contiguous sections of board.

2 painters cannot share a board to paint. That is to say, a board
cannot be painted partially by one painter, and partially by another.
A painter will only paint contiguous boards. Which means a
configuration where painter 1 paints board 1 and 3 but not 2 is
invalid.
Return the ans % 10000003

Input :
K: Number of painters
T: Time taken by painter to paint 1 unit of board
L: A List which will represent length of each board

Output:
     return minimum time to paint all boards % 10000003
Example

Input: 
  K: 2
  T: 5
  L: [1, 10]
Output: 50

# Hint 1

If you had a function bool isPossible which could tell you if its possible to paint the boards in time T or less, can you solve the problem ?

# Hint 2

If you have already solved the problem corresponding to hint1, you are already halfway there.

You can do a binary search for the answer :

  start = 0, end = max_time_possible
  mid = (start + end) / 2
  if isPossible(mid): 
    end = mid - 1
  else 
    start = mid + 1
Now, lets look into how isPossible would be implemented. 
Keep assigning boards to painter greedily till the time taken < mid. If you run out of painters, isPossible = false. 
else isPossible = true.

## Solution

```cpp

/* tutorial */

int split(vector<long> &sum, long target) {
    int cnt=0;
    auto it=sum.begin()+1;
    while (it!=sum.end()) {
        it--;
        it=upper_bound(it+1,sum.end(),*it+target);
        cnt++;
    }
    return cnt;
}

int Solution::paint(int A, int B, vector<int> &C) {
    vector<long> sum(C.size()+1,0);
    long lo(INT_MIN), hi;
    for (int i=0; i<C.size(); ++i) {
        lo=max(lo,(long)C[i]);
        sum[i+1]=C[i]+sum[i];
    }
    hi=sum.back();
    while (lo<hi) {
        long mid = lo+(hi-lo)/2;
        if (split(sum,mid)>A) lo=mid+1;
        else hi=mid;
    }
    return (lo*B)%10000003;
}


/* another solution */

bool isPossible(long long _Max, int K, vector<int> &arr) {
             
    int _max_ele = *max_element(arr.begin(), arr.end());
    if(_max_ele > _Max)
            return false; 
             
    long long Sum = 0;
    int cnt = 1;
    int N = arr.size();
 
    for(int i = 0; i < N;) {
        if(Sum + ((long long)arr[i]) > _Max) {
            Sum = 0;
            cnt++;
        } else {
            Sum += ((long long)arr[i]);
            i++;
        }
    }
 
    if(cnt <= K) return true;
    return false;
}
 
int Solution::paint(int A, int B, vector<int> &C) {    
    int N = C.size();
             
    long long end = 0;
    long long start = 0;
 
    for(int i = 0; i < N; ++i) {
        end += C[i];
    }
     
    long long ans = INT_MAX;
    ans *= INT_MAX;
    while(start <= end) {
         
        long long mid = (start + end) / 2;
        if(isPossible(mid, A, C)) {
            ans = min(ans, mid);
            end = mid - 1;
        } 
        else {
            start = mid + 1;
        }
    }
             
    return (ans * (long long) B) % 10000003;
}
```