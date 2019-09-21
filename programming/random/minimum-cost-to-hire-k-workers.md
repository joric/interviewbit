# Minimum Cost to Hire K Workers

https://www.interviewbit.com/problems/minimum-cost-to-hire-k-workers/

Given two array of integers of A and B describing the quality and minimum wage expectations of N workers.

Now, you want to hire exactly C workers to form a paid group.
When hiring a group of C workers, we must pay them according to the following rules:

Every worker in the paid group should be paid in the ratio of their quality compared to other workers in the paid group.

Every worker in the paid group must be paid at least their minimum wage expectation.

let D be the least amount of money needed to form a paid group satisfying the above conditions.

D can be any real value.

Return the ceil(D).

ceil(x): Returns the smallest integer that is greater than or equal to x (i.e: rounds up the nearest integer).

### Input Format

```
The first argument given is the integer array A.
The second argument given is the integer array B.
The third argument given is the integer C.
```

### Output Format

Return the ceil(D).

### Constraints
```
1 <= N <= 100000
1 <= A[i], B[i] <= 10000 
1 <= C <= N
```
### For Example

```
Input 1:
    A = [10, 20, 5]
    B = [70, 50, 30]
    C = 2
Output 1:
    105
    Explanation 1:
        Pay 70 to the first worker
        Pay 35 to the third worker
        Total = 70 + 35 = 105
        
Input 2:
    A = [3, 1, 10, 10, 1]
    B = [4, 8, 2, 2, 7]
    C = 3
Output 2:
    31
    Explanation 2:
        Pay 4 to the first worker
        Pay 13.33333 to the third worker
        Pay 13.33333 to the fourth worker
        Total = 4 + 13.33333 + 13.333333 = 30.66667
        ceil(30.66667) = 31
```
## Hint 1

At least one worker will be paid their minimum wage expectation. If not, we could scale all payments down by some factor and still keep everyone earning more than their wage expectation.

## Solution Approach

Additionally, every worker has some minimum ratio of dollars to quality that they demand.
For example, if wage[0] = 100 and quality[0] = 20, then the ratio for worker 0 is 5.0.

The key insight is to iterate over the ratio. Let’s say we hire workers with a ratio R or lower.
Then, we would want to know the K workers with the lowest quality, and the sum of that quality.
We can use a heap to maintain these variables.

## Solution
### Editorial
```cpp
static bool compare(const pair<double,int> &w1, const pair<double,int> &w2) {
        return w1.first < w2.first;
    }

double mincostToHireWorkers(vector<int>& quality, vector<int>& wage, int K) {
    priority_queue<int> quality_heap;
    vector<pair<double,int>> worker;
    for(int i=0; i< quality.size(); i++)
        worker.push_back(make_pair(double(wage[i])/quality[i],quality[i]));
    sort(worker.begin(),worker.end(), compare);
    double ratio = 0;
    int total_quality = 0;
    double min_cost = 1.0*INT_MAX;
    for(auto w: worker) {
        if(quality_heap.size() == K && w.second < quality_heap.top()) {
            total_quality -= quality_heap.top();
            quality_heap.pop();
        }
        if(quality_heap.size() < K) {
            quality_heap.push(w.second);
            total_quality+=w.second;
            ratio = w.first;
            if(quality_heap.size() == K)
                min_cost = min(ratio*total_quality, min_cost);
        }
    }
    assert(min_cost>=0&&min_cost<=2000000000.00);
    return min_cost;
}

int Solution::solve(vector<int> &A, vector<int> &B, int C) {
    double ans=mincostToHireWorkers(A,B,C);
    return ceil(ans);
}
```
### Fastest
```cpp
struct Worker {
    int quality;
    int wages;
    double unit_cost; // That's the unit cost 
    Worker(int q, int w): quality(q), wages(w), unit_cost((double)(w)/(double)(q)) {};
    bool operator<(const Worker&other) const {
        return unit_cost < other.unit_cost;
    }
};
int Solution::solve(vector<int> &A, vector<int> &B, int C) {
    double res = numeric_limits<double>::max();
    vector<Worker> workers;
    for(int i=0;i<A.size();i++){
        workers.emplace_back(Worker(A[i], B[i]));
    }
    sort(workers.begin(),workers.end());
    priority_queue<int, vector<int>, less<int>> pq;
    int sumq=0;
    for(const Worker& w: workers){
        pq.push(w.quality);
        sumq+=w.quality;
        if(pq.size()>C){
            sumq-=pq.top();
            pq.pop();
        }
        if(pq.size()==C){
            res=min(res,w.unit_cost*sumq);
        }
    }
    return ceil(res);
}
```
### Lightweight
```cpp
int Solution::solve(vector<int> &A, vector<int> &B, int C) {
    int n = A.size();
    vector<int> idx(n);
    iota(idx.begin(), idx.end(), 0);
    sort(idx.begin(), idx.end(), [&](int i, int j) {
        return B[i]*1LL*A[j]<B[j]*1LL*A[i];
    });
    
    priority_queue<int> q;
    long sum = 0;
    for (int i = 0; i < C; ++i) {
        int j = idx[i];
        q.push(A[j]);
        sum += A[j];
    }
    
    long res = (B[idx[C-1]]*sum + A[idx[C-1]] - 1)/A[idx[C-1]];
    for (int i = C; i < n; ++i) {
        int x = q.top(), j = idx[i];
        q.pop();
        sum -= x;
        q.push(A[j]);
        sum += A[j];
        long r = (B[j]*sum + A[j] - 1)/A[j];
        if (r<res)res=r;
        q.push(x);
        sum += x;
        x = q.top();
        q.pop();
        sum -= x;
    }
    
    return res;
}
```

### Mine (python)
```python
import heapq
import math
class Solution:
    def solve(self, quality, wage, K):
        ratio = []
        in_k, out_k = [],[]
        import heapq as hq
        hq.heapify(in_k) # min ratio
        hq.heapify(out_k) # highest quality
        for i in range(len(quality)):
            r = wage[i]/quality[i]
            ratio.append(r)
            hq.heappush(out_k,(r,i))
        i,maxr = K,0
        sumq = 0
        while i > 0:
            _,ind = hq.heappop(out_k)
            hq.heappush(in_k,(-quality[ind],ind))
            maxr = ratio[ind]
            sumq += quality[ind]
            i -= 1
        minw = [sumq*maxr]
        while out_k:
            maxr, ind = hq.heappop(out_k)
            minus, mind = hq.heappop(in_k)
            hq.heappush(in_k, (-quality[ind],ind))
            sumq += minus+quality[ind]
            minw.append(maxr*sumq)
        return math.ceil(min(minw))
```

