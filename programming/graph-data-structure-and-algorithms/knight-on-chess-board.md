# Knight On Chess Board
https://www.interviewbit.com/problems/knight-on-chess-board/

Knight movement on a chess board

Given any source point and destination point on a chess board, we need to find whether Knight can move to the destination or not.

![](http://i.imgur.com/lmKL4AU.jpg)

Knight's movements on a chess board

The above figure details the movements for a knight ( 8 possibilities ). Note that a knight cannot go out of the board.

If yes, then what would be the minimum number of steps for the knight to move to the said point.
If knight can not move from the source point to the destination point, then return -1

Input:
```
N, M, x1, y1, x2, y2
where N and M are size of chess board
x1, y1  coordinates of source point
x2, y2  coordinates of destination point
```

Output:

```
return Minimum moves or -1
```
Example

```
Input : 8 8 1 1 8 8
Output :  6
```

## Hint 1
Assume this problem as searching in graph where each block of chess board is vertex. 
How would you define edges in such a graph ? 
When can you travel from vertex i to vertex j ?

Once you have the graph, then it reduces to finding the shortest path in an unweighted graph. 
How do you find the shortest path in an unweighted graph ?
## Solution Approach
```
A knight can move to 8 positions from (x,y). 

(x, y) -> 
    (x + 2, y + 1)  
    (x + 2, y - 1)
    (x - 2, y + 1)
    (x - 2, y - 1)
    (x + 1, y + 2)
    (x + 1, y - 2)
    (x - 1, y + 2)
    (x - 1, y - 2)

Corresponding to the knight's move, we can define edges. 
(x,y) will have an edge to the 8 neighbors defined above. 

To find the shortest path, we just run a plain BFS. 
```

## Solution

```cpp
vector<int> x ({1, 2, 2, 1, -1, -2, -1, -2});
vector<int> y ({2, 1, -1, -2, 2, 1, -2, -1});

bool isSafe(int i , int j ,int N, int M){
    if(i >= 0 && i < N && j >= 0 && j < M)return true;
    
    return false;
}

int explore(queue<pair<int, pair<int, int> > >& q, int A, int B, int C, int D, int E, int F, vector<vector<int> >& visited, int &count){
    while(!q.empty()){
        int distance = q.front().first;
        int c = q.front().second.first;
        int d = q.front().second.second;
        q.pop();
        if ( c == E && d == F)return distance;
        visited[c][d] = 1;

        for(int i = 0; i < x.size(); i++){
            int first = c + x[i];
            int second = d + y[i];
            if(isSafe(first, second, A, B)){
                if(visited[first][second] == 0){
                    visited[first][second] = 1;
                    q.push({distance+1, {first, second}});
                }
            }
        }
    }
    return -1;
}

int Solution::knight(int A, int B, int C, int D, int E, int F) {
    vector<vector<int> > visited (A, vector<int> (B, 0));
    
    int count  = 0;
    queue<pair<int, pair<int, int> > > q;
    q.push({0, {C-1, D-1}});
    return explore(q, A, B, C-1, D-1, E-1, F-1, visited, count);
}

```

## Asked in
* Goldman Sachs
* Amazon

