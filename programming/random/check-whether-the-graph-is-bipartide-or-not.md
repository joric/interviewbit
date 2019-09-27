# Check whether the graph is bipartide or not

https://www.interviewbit.com/problems/check-whether-the-graph-is-bipartide-or-not/

Given a undirected graph having A nodes.
A matrix B of size M x 2 is given which represents the edges such that there is an edge between
`B[i][0] and B[i][1]`.

Find whether the given graph is bipartide or not.

A graph is bipartite if we can split it's set of nodes into two independent subsets A and B such that every edge in the graph has one node in A and another node in B

### Note:

* There are no self-loops in the graph.
* No multiple edges between two pair of vertices.
* The graph may or may not be connected.
* Nodes are Numbered from 0 to A-1.
* Your solution will run on multiple testcases. If you are using global variables make sure to clear them.


### Input Format

* The first argument given is an integer A.
* The second argument given is the matrix B.

### Output Format

* Return 1 if the given graph is bipartide else return 0.

### Constraints
```
1 <= A <= 100000
1 <= M <= min(A*(A-1)/2,200000)
0<= B[i][0],B[i][1] < A
```
### For Example
```
Input 1:
    A : 10
    B : [   [7, 8]
            [1, 2]
            [0, 9]
            [1, 3]
            [6, 7]
            [0, 3]
            [2, 5]
            [3, 8]
            [4, 8]  ]
Output 1:
    1

Input 2:
    A : 9
    B : [   [2, 5]
            [6, 7]
            [4, 8]
            [2, 3]
            [0, 3]
            [4, 7]
            [1, 8]
            [3, 8]
            [1, 3]  ]
Output 2:
    0
```

## Hint 1

Try to color the graph using two colors.
If possible then it is bipartite else not.


## Solution approach

You can use and approach either BFS or DFS to check whether the graph can be colored using two colors or not.

Start from any node that hase not been colored yet:

a. Assign color1 to this nodes
b. check its adjacent nodes
a. if this is colored in color1 then the graph can’t be bipartite ,return 0.
b. else if this is colored in color1 do nothing.
c. else color it with color 2 and push it into queue.

Repeat step1 until no nodes is left for coloring.


## Solution
### Editorial
```cpp
const int N =100005;
vector<int> graph[N];


void clean(int n){
    for(int i=0; i<=n; ++i)
        graph[i].clear();
}

void make_graph(int n,vector<vector<int> > &edges){
    clean(n);
    for(int i=0; i<edges.size();++i){
        graph[edges[i][0]].push_back(edges[i][1]);
        graph[edges[i][1]].push_back(edges[i][0]);
    }
}

bool isBipartite(int n) {
    if(!n)
        return true;
    int color[n];
    memset(color,-1,sizeof color);
    queue<int> q;
    for(int i=0; i<n; ++i){
        if(color[i]!=-1)
            continue;
        color[i]=1;
        q.push(i);
        while(!q.empty()){
            int x=q.front();
            q.pop();
            for(auto &it:graph[x]){
                if(color[it]==-1){
                    color[it]=color[x]^1;
                    q.push(it);
                }
                else if(color[it]==color[x])
                    return false;
                }
        }
    }
    return true;
}


int Solution::solve(int A, vector<vector<int> > &B) {
    make_graph(A,B);
    if(isBipartite(A))
        return 1;
    return 0;
}
```

### Lightweight
```cpp
#define eb emplace_back
int Solution::solve(int A, vector<vector<int> > &B) {
    vector<int> v[A];
    for(int i=0;i<B.size();i++){
        int a = B[i][0],b=B[i][1];
        //a--,b--;
        v[a].eb(b);
        v[b].eb(a);
    }
    int n=A;
    int vis[n],color[n];
    for(int i=0;i<n;i++){
        color[i]=-1;
    }
    for(int i=0;i<n;i++){
        if(color[i]!=-1) continue;
        queue<int> q;
        q.push(i);
        color[i]=0;
        while(!q.empty()){
            auto u =  q.front();
            q.pop();
            for(auto j : v[u]){
                if(color[j] == -1){
                    color[j] = 1-color[u];
                    q.push(j);
                }
                if(color[j]==color[u]){
                    return 0;
                }
            }
        }
    }
    return 1;
}

```




