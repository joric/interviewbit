# Commutable Islands
https://www.interviewbit.com/problems/commutable-islands/

There are n islands and there are many bridges connecting them. Each bridge has some cost attached to it.

We need to find bridges with minimal cost such that all islands are connected.

It is guaranteed that input data will contain at least one possible scenario in which all islands are connected with each other.

### Example

Input
```
Number of islands ( n ) = 4
1 2 1
2 3 4
1 4 3
4 3 2
1 3 10
```
In this example, we have number of islands(n) = 4 . Each row then represents a bridge configuration. In each row first two numbers represent the islands number which are connected by this bridge and the third integer is the cost associated with this bridge.

In the above example, we can select bridges 1(connecting islands 1 and 2 with cost 1), 3(connecting islands 1 and 4 with cost 3), 4(connecting islands 4 and 3 with cost 2). Thus we will have all islands connected with the minimum possible cost(1+3+2 = 6). 
In any other case, cost incurred will be more.

## Hint 1

Hint: Try to think in terms of graph.
Can we make a graph out of the given data and use some graph algorithms?

## Solution Approach

We can assume islands as the vertex points and bridges as the edges and can construct a graph with the the help of them. After constructing the graph, the problem boils down to finding a subset of edges which helps in connecting vertices in a single connected component such that the sum of their edge weights is as minimum as possible.

Now since the problem is clear to you, can you think of any graph theory algorithms related to this?

Well the answer to this problem is the classic minimum spanning tree algorithm. In this algorithm we aim at finding subset of the edges of a connected, edge-weighted undirected graph that connects all the vertices together, without any cycles and with the minimum possible total edge weight.

There are many algorithms for finding minimum spanning tree of a graph. Some of them are Kruskal's algorithm, Prim's algorithm etc.

Kruskal's algorithm in detail can be found at: https://en.wikipedia.org/wiki/Kruskal%27s_algorithm

Prim's algorithm in detail can be found at: https://en.wikipedia.org/wiki/Prim%27s_algorithm

Now, can you code this?

## Solution
```cpp
int Solution::solve(int N, vector<vector<int>>& bridges) {
    vector<vector<pair<int,int>>> g(N+1);
    for(auto& bridge: bridges){
        g[bridge[0]].push_back(make_pair(bridge[2], bridge[1]));
        g[bridge[1]].push_back(make_pair(bridge[2], bridge[0]));
    }
    
    priority_queue<pair<int,int>, vector<pair<int,int>>, greater<pair<int,int>>> q;//minheap
    unordered_set<int> visited;
    int totalCost = 0;
    q.push(make_pair(0,1));
    while(!q.empty()){
        auto curr = q.top();
        q.pop();
        if(visited.find(curr.second)!=visited.end()) continue;
        visited.insert(curr.second);
        
        totalCost += curr.first;
        for(auto& adj: g[curr.second]){
            if(visited.find(adj.second)!=visited.end()) continue;
            q.push(adj);
        }
    }
    return totalCost;
}
```
