# Largest Distance between nodes of a Tree

https://www.interviewbit.com/problems/largest-distance-between-nodes-of-a-tree/

## Find largest distance

Given an arbitrary unweighted rooted tree which consists of N (2 <= N <= 40000) nodes.
The goal of the problem is to find largest distance between two nodes in a tree.
Distance between two nodes is a number of edges on a path between the nodes
(there will be a unique path between any pair of nodes since it is a tree).
The nodes will be numbered 0 through N - 1.

The tree is given as an array P, there is an edge between nodes P[i] and i (0 <= i < N).
Exactly one of the i's will have P[i] equal to -1, it will be root node.

Example:

```
If given P is [-1, 0, 0, 0, 3], then node 0 is the root and the whole tree looks like this:

          0
       /  |  \
      1   2   3
               \
                4  
```
One of the longest path is 1 -> 0 -> 3 -> 4 and its length is 3, thus the answer is 3.
Note that there are other paths with maximal distance

## Hint 1

How would you solve the problem if you knew the longest path certainly goes through root? Try to generalize this approach for other nodes.

## Solution Approach
```
1) pick any node u
2) find the node which is farthest from u, call it x (calculate using the same approach as in Solution 1)
3) find the node which is farthest from x, call it q (calculate using the same approach as in Solution 1)
The answer will be the length of a path from x to q.

Proof of correctness:

The crucial step is to prove that x will be one of the endpoints of
the path with maximal length (note that there might be more than one such path).
If it is, then the longest path from x will be the longest path in the tree.

Let d(v1, v2) be length of path between v1 and v2

Let's prove it by contradiction: assume there is a strictly longer path between s and t,
neither of which is x. Let h be a node which is closest to u among the nodes on a path between s and t.
Then there are two cases:

1) h is on path between u and x

    u
    |
    |
    |
    h   x
   / \ /
  /   *
 /     \
s       t 
then d(s, t) = d(s, h) + d(h, t) <= d(s, h) + d(h, x) = d(s, x), which contradicts assumption.

2) h is not on path between u and x

    u
    |
    *---x
    |
    h
   / \
  /   \
 /     \
s       t

then

d(u, s) <= d(u, x) <= d(u, h) + d(h, x)
d(u, t) <= d(u, x) <= d(u, h) + d(h, x)

d(s, t) = d(s, h) + d(h, t)
= d(u, s) + d(u, t) - 2 d(u, h)
<= 2 d(h, x)

2 d(s, t) <= d(s, t) + 2 d(h, x)
= d(s, h) + d(h, x) + d(x, h) + d(h, t)
= d(x, s) + d(x, t)

This means that max(d(v, s), d(v, t)) >= d(s, t), which also contradicts assumption.

Thus, we proved that farthest node of a node will be one of the endpoints of the longest path.
```

## Solution

### Editorial
```cpp
int max_ans;
int dfs(int src,vector<int>tree[]) {
    int max1=0,max2=0;
    int size=tree[src].size();//no of childs of src
    if(size==0)//this node will contribute 1 edge when linked with its parent
        return 1;
    for(int i=0;i<size;i++) {
        int ans=dfs(tree[src][i],tree);
        if(ans>max1) {
            max2=max1;
            max1=ans;
        } else if(ans>max2)
            max2=ans;
    }
    max_ans=max(max_ans,max1+max2);
    return 1+max(max1,max2);
}

int Solution::solve(vector<int> &A) {
    max_ans=0;
    int n=A.size();
    if(n<2)
        return 0;

    //nodes will be 0 to n-1
    int root=-1;
    vector<int>tree[n];

    for(int i=0;i<n;i++) {
        if(A[i]==-1) {
            root=i;
            continue;
        }
        int src=A[i],dest=i;
        tree[src].push_back(dest);
    }
    //now start dfs with root
    dfs(root,tree);
    return max_ans;
}

```

### Fastest

```cpp
int Solution::solve(vector<int> &A) {
        vector<int> res1;
    vector<int> res2;
    res1.resize(A.size());
    res2.resize(A.size());
    for(int i=A.size()-1;i>0;i--){
        if(res1[A[i]]<max(res1[i],res2[i])+1){
            res2[A[i]]=max(res2[A[i]],res1[A[i]]);
            res1[A[i]]=max(res1[i],res2[i])+1;
        }
        else if(res2[A[i]]<max(res1[i],res2[i])+1)
            res2[A[i]]=max(res1[i],res2[i])+1;
    }
    int r=0;
    for(int i=0;i<A.size();i++){
        //cout<<res1[i]<<" "<<res2[i]<<endl;
        r=max(r,res1[i]+res2[i]);
    }
    return r;

}
```

### Mine

```cpp
pair<int, int> process(queue<pair<int, int> >& q, vector<int>& visited, unordered_map<int, vector<int> >& x){
    // Typical BFS with keeping track of the longest distance and farthest element.
    int maxi = INT_MIN, farthest = 0;
    while(!q.empty()){
        int node = q.front().first;
        int distance = q.front().second;
        q.pop();
        if(distance > maxi){
            maxi = distance;
            farthest = node;
        }
        visited[node] = 1;
        for(int i = 0; i < x[node].size(); i++){
            if(!visited[x[node][i]]){
                visited[x[node][i]] = 1;
                q.push({x[node][i], distance+1});
            }
        }
    }
    return {maxi, farthest};
}

int Solution::solve(vector<int> &A) {
    // Base Case
    if(A.size() <= 1)return 0;
    
    // Visited array to keep track of visited elements
    vector<int> visited(A.size(), 0);
    
    // Map to make the graph from given array
    unordered_map<int, vector<int> > x;
    x.clear();
    
    for(int i = 0; i < A.size(); i++){
        // If the value is -1, it doesn't have any parent.
        // Make the pairs in the adj. list type map
        if( A[i] != -1){
            x[A[i]].push_back(i);
            x[i].push_back(A[i]);
        }
    }
    
    // Start from the first element in the array and mark it visited.
    queue<pair<int, int> > q;
    q.push({0, 0});
    visited[0] = 1;

    // Gets the farthest element from the first element
    int farthest = process(q, visited, x).second;

    vector<int> vis(A.size(), 0);
    
    // Now apply BFS on the farthest element found and return the distance
    // of the farthest element found so far.
    q.push({farthest, 0});
    vis[farthest] = 1;
    return process(q, vis, x).first;
}

```

## Asked in

* Facebook
* Google
