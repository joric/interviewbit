# Rod Cutting

https://www.interviewbit.com/problems/rod-cutting/

There is a rod of length N lying on x-axis with its left end at x = 0 and right end at x = N. Now, there are M weak points on this rod denoted by positive integer values(all less than N) A1, A2, …, AM. You have to cut rod at all these weak points. You can perform these cuts in any order. After a cut, rod gets divided into two smaller sub-rods. Cost of making a cut is the length of the sub-rod in which you are making a cut.

Your aim is to minimise this cost. Return an array denoting the sequence in which you will make cuts. If two different sequences of cuts give same cost, return the lexicographically smallest.

### Notes
```
Sequence a1, a2 ,…, an is lexicographically smaller than b1, b2 ,…, bm, if and only if at the first i where ai and bi differ, ai < bi, or if no such i found, then n < m.
N can be upto 109.
For example,

N = 6
A = [1, 2, 5]

If we make cuts in order [1, 2, 5], let us see what total cost would be.
For first cut, the length of rod is 6.
For second cut, the length of sub-rod in which we are making cut is 5(since we already have made a cut at 1).
For third cut, the length of sub-rod in which we are making cut is 4(since we already have made a cut at 2).
So, total cost is 6 + 5 + 4.

Cut order          | Sum of cost
(lexicographically | of each cut
 sorted)           |
___________________|_______________
[1, 2, 5]          | 6 + 5 + 4 = 15
[1, 5, 2]          | 6 + 5 + 4 = 15
[2, 1, 5]          | 6 + 2 + 4 = 12
[2, 5, 1]          | 6 + 4 + 2 = 12
[5, 1, 2]          | 6 + 5 + 4 = 15
[5, 2, 1]          | 6 + 5 + 2 = 13
```

So, we return [2, 1, 5].

## Hint 1

Are you convinced yet any type of greedy solution won’t work? If not, try some examples. 

Let’s say from the set of cuts A1, A2, …, AN, first cut you make at is Ai.

Then, we have to make cuts from set A1, A2, …, Ai-1, whose order doesn’t depend on set of cuts Ai+1, Ai+2, …, AN.

Can you think this in terms of dynamic programming?


## Solution Approach

We rewrite our problem as given N cut points(and you cannot make first and last cut), decide order of these cuts to minimise the cost. So, we insert 0 and N at beginning and end of vector B. Now, we have solve our new problem with respect to this new array(say A).

We define dp(i, j) as minimum cost for making cuts Ai, Ai+1, …, Aj. Note that you are not making cuts Ai and Aj, but they decide the cost for us.

For solving dp(i, j), we iterate k from i+1 to j-1, assuming that the first cut we make in this interval is Ak. The total cost required(if we make first cut at Ak) is Aj - Ai + dp(i, k) + dp(k, j).

This is our solution. We can implement this DP recursively with memoisation. Total complexity will be O(N3).

For actually building the solution, after calculating dp(i, j), we can store the index k which gave the minimum cost and then we can build the solution backwards.

## Solution

### Editorial
```cpp


typedef long long LL;

//ans vector
vector<int> ans;

//cuts vector
vector<int> ar;

//dp array
vector<vector<LL> > dp;

//parent array
vector<vector<int> > parent;

//solve for dp(l, r)
LL rec(int l, int r){
    //base case
    if(l+1>=r)return 0;

    //for memoisation
    LL &ret=dp[l][r];

    if(ret!=-1)return ret;


    ret=LLONG_MAX;
    int bestind;    //stores the best index

    for(int i=l+1; i<r; i++){
        //recurrence
        LL p=rec(l,i)+rec(i,r) + (LL)ar[r]-(LL)ar[l];

        //update best
        //note that we choose lexicographically smallest index
        //if multiple give same cost
        if(p<ret){
            ret=p;
            bestind=i;
        }
    }

    //store parent of (l, r)
    parent[l][r]=bestind;

    return ret;
}

//function for building solution
void back(int l, int r){
    //base case
    if(l+1>=r)return;

    //first choose parent of (l,r)
    ans.push_back(ar[parent[l][r]]);

    //call back recursively for two new segments
    //calling left segment first because we want lexicographically smallest
    back(l,parent[l][r]);
    back(parent[l][r],r);
}

vector<int> Solution::rodCut(int A, vector<int> &B) {
    //insert A and 0
    B.push_back(A);
    B.insert(B.begin(),0);


    int n=B.size();
    ar.clear();
    for(int i=0; i<n; i++)
        ar.push_back(B[i]);

    //initialise dp array
    dp.resize(n);
    parent.resize(n);
    ans.clear();
    for(int i=0; i<n; i++){
        dp[i].resize(n);
        parent[i].resize(n);
        for(int j=0; j<n; j++)
            dp[i][j]=-1;
    }

    //call recurrence
    LL best=rec(0,n-1);

    //build solution
    back(0,n-1);

    return ans;
}

```

### Fastest
```cpp
typedef struct stelem
{
    int cutPos;
    int start;
    int end;
}stelem;

vector<int> Solution::rodCut(int A, vector<int> &B) {
    
    int i, l, k, size;
    sort(B.begin(), B.end());
    vector<int> :: iterator it = B.begin();
    B.insert(it, 0);
    B.push_back(A);
    size = B.size();
    
    vector<vector<pair<long long, int> > > memo(size, vector<pair<long long, int> >(size, make_pair(0, -1)));

    // memo[i][j] = min cost of cutting a rod starting from i to j
    // = (j-i) + min (memo[i][k] + min[k][j]) where k is each cut b/w i and j
    // memo[0][A] = min cost of cutting the complete rod

    // Fill based on the number of cuts starting 1
    for(l=0; l<size-1; l++)
    {
        for (i=l+1; i<size; i++)
        {
            // start = i-l-1, end = i;
            int s = i-l-1;
            int e = i;

            if (l == 0)
            {
                memo[s][e].first = 0;
                memo[s][e].second = -1;
            }
            else
            {
                // k = start+1 to end-1
                long long temp = LONG_MAX, val;
                int cutPos = -1;
                for (k = s+1; k < e; k++)
                {
                    val = memo[s][k].first + memo[k][e].first;
                    if (temp > val)
                    {
                        temp = val;
                        cutPos = k;
                    }
                }
                memo[s][e].first = (long long)(B[e]-B[s]) + temp;
                memo[s][e].second = cutPos;
            }
        }
    }

    // Figure out the cut sequence
    stack<stelem> st;
    vector<int> result;
    // push first stelem
    if (memo[0][size-1].second != -1)
    {
        stelem t;
        t.cutPos = memo[0][size-1].second;
        t.start = 0;
        t.end = size-1;
        st.push(t);
    }

    while(!st.empty())
    {
        // pop top element
        stelem temp = st.top();
        st.pop();
        result.push_back(B[temp.cutPos]);

        // Push right part (cutPos - End)
        if (memo[temp.cutPos][temp.end].second != -1)
        {
            stelem right;
            right.cutPos = memo[temp.cutPos][temp.end].second;
            right.start = temp.cutPos;
            right.end = temp.end;
            st.push(right);
        }

        // Push left part (start - cutPos)
        if (memo[temp.start][temp.cutPos].second != -1)
        {
            stelem left;
            left.cutPos = memo[temp.start][temp.cutPos].second;
            left.start = temp.start;
            left.end = temp.cutPos;
            st.push(left);
        }
    }

    return result;
}
```

### Lightweight
```cpp
void getAns(vector<int> &ans, vector<vector<int>> &dp, int i, int j, vector<int> p) {
    if (i + 1 == j) return ;
    for (int k = i + 1; k < j; k++) {
        if (dp[i][k] + dp[k][j] + p[j] - p[i] == dp[i][j]) {
            ans.push_back(p[k]);
            getAns(ans, dp, i, k, p);
            getAns(ans, dp, k, j, p);
            return ;
        }
    }
    return ;
}
vector<int> Solution::rodCut(int A, vector<int> &B) {
    vector<int> p;
    p.push_back(0);
    for (int b: B) p.push_back(b);
    p.push_back(A);
    sort(p.begin(), p.end());
    
    int n = p.size();
    vector<vector<int>> dp(n, vector<int>(n, 0));
    for (int l = 2; l <= n; l++) {
        for (int i = 0; i + l - 1 < n; i++) {
            dp[i][i+l-1] = INT_MAX;
            for (int k = i + 1; k < i + l - 1; k++)
                dp[i][i+l-1] = min(dp[i][i+l-1], dp[i][k] + dp[k][i+l-1] + p[i+l-1] - p[i]);
        }
                
    }
    
    vector<int> ans;
    getAns(ans, dp, 0, n - 1, p);
    return ans;
    
}

```

## Asked in
* Google

