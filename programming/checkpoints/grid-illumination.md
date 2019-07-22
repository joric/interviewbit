# Grid Illumination

https://www.interviewbit.com/problems/grid-illumination/

You are given an integer A.
On a A x A grid of cells, each cell (x, y) with 0 <= x < A and 0 <= y < A has a lamp.

Initially, some number of lamps are on. B[i] tells us the location of the i-th lamp that is on. Each lamp that 
is on illuminates every square on its x-axis, y-axis, and both diagonals (similar to a Queen in chess).

For the i-th query C[i] = (x, y), the answer to the query is 1 if the cell (x, y) is illuminated, else 0.

After each query (x, y) [in the order given by queries], we turn off any lamps that are at cell (x, y) or 
are adjacent 8-directionally (ie., share a corner or edge with cell (x, y).)

Return an array of D. Each value D[i] should be equal to the answer of the i-th query C[i].

```
Input Format

The first argument given is the integer A.
The second argument given is the integer matrix B.
The third argument given is the integer matrix C.
Output Format

Return an array of integers D. Each value D[i] should be equal to the answer of the i-th query C[i]
Constraints

1 <= A <= 10^9
1 <= B.length , C.length <= 20000
B[i].length = C[i].length = 2
For Example

Input 1:
    A = 5
    B = [[0,4], [4,4]]
    C = [[1,1], [1,0]]
Output 1:
    D = [1, 0]

Input 2:
    A = 6
    B = [[1,3], [2,4], [5,4]]
    C = [[2,4], [1,2]]
Output 2:
    D = [1, 0]
```
## Solution

### Editorial
```cpp
vector<int> gridIllumination(int n, vector<vector<int>> &lamps, vector<vector<int>> &queries) {
    vector<int> retv;
    // like N queen question . in this part we use  four map,each map have his idea
    long long N = n;
    unordered_map<long, unordered_set<long long>> row, col, dia1, dia2; //dia1 means x-y = constant
    for (auto lamp : lamps) {
        row[lamp[0]].insert(N * lamp[0] + lamp[1]);
        col[lamp[1]].insert(N * lamp[0] + lamp[1]);
        dia1[lamp[0] - lamp[1]].insert(N * lamp[0] + lamp[1]);
        dia2[lamp[0] + lamp[1]].insert(N * lamp[0] + lamp[1]);
    }
    vector<vector<int>> dirs = { { -1, -1 }, { -1, 0 }, { -1, 1 }, { 0, -1 }, { 0, 0 }, { 0, 1 }, { 1, -1 }, { 1, 0 }, { 1, 1 } };
    for (auto query : queries) {
        long x = query[0];
        long y = query[1];
        if (row.count(x) || col.count(y) || dia1.count(x - y) || dia2.count(x + y)) {
            retv.push_back(1);
        } else {
            retv.push_back(0);
        }
        for (auto dir : dirs) {
            int dx = dir[0];
            int dy = dir[1];
            int dir_x = x + dx;
            int dir_y = y + dy;
            //four dir
            //row
            if (row.count(dir_x) && row[dir_x].count(N * dir_x + dir_y)) {
                //erase lamp
                row[dir_x].erase(N * dir_x + dir_y);
                if (row[dir_x].size() == 0) {
                    row.erase(dir_x);
                }
            }
            if (col.count(dir_y) && col[dir_y].count(N * dir_x + dir_y)) {
                //erase lamp
                col[dir_y].erase(N * dir_x + dir_y);
                if (col[dir_y].size() == 0) {
                    col.erase(dir_y);
                }
            }
            if (dia1.count(dir_x - dir_y) && dia1[dir_x - dir_y].count(N * dir_x + dir_y)) {
                //erase lamp
                dia1[dir_x - dir_y].erase(N * dir_x + dir_y);
                if (dia1[dir_x - dir_y].size() == 0) {
                    dia1.erase(dir_x - dir_y);
                }
            }
            if (dia2.count(dir_x + dir_y) && dia2[dir_x + dir_y].count(N * dir_x + dir_y)) {
                //erase lamp
                dia2[dir_x + dir_y].erase(N * dir_x + dir_y);
                if (dia2[dir_x + dir_y].size() == 0) {
                    dia2.erase(dir_x + dir_y);
                }
            }
        }
    }
    return retv;
}

vector<int> Solution::solve(int A, vector<vector<int>> &B, vector<vector<int>> &C) {

    return gridIllumination(A, B, C);
}

```

### Fastest
```cpp
vector<int> Solution::solve(int N, vector<vector<int>> &lamps, vector<vector<int>> &queries) {
    unordered_set<long> s;
    unordered_map<int, int> lx, ly, lp, lq;
    for (const auto &lamp : lamps) {
        const int x = lamp[0];
        const int y = lamp[1];
        s.insert(static_cast<long>(x) << 32 | y);
        ++lx[x];
        ++ly[y];
        ++lp[x + y];
        ++lq[x - y];
    }
    vector<int> ans;
    for (const auto &query : queries) {
        const int x = query[0];
        const int y = query[1];
        if (lx.count(x) || ly.count(y) || lp.count(x + y) || lq.count(x - y)) {
            ans.push_back(1);
            for (int tx = x - 1; tx <= x + 1; ++tx)
                for (int ty = y - 1; ty <= y + 1; ++ty) {
                    if (tx < 0 || tx >= N || ty < 0 || ty >= N) continue;
                    const long key = static_cast<long>(tx) << 32 | ty;
                    if (!s.count(key)) continue;
                    s.erase(key);
                    if (--lx[tx] == 0) lx.erase(tx);
                    if (--ly[ty] == 0) ly.erase(ty);
                    if (--lp[tx + ty] == 0) lp.erase(tx + ty);
                    if (--lq[tx - ty] == 0) lq.erase(tx - ty);
                }
        } else {
            ans.push_back(0);
        }
    }
    return ans;
}

```
