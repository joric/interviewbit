# Valid Path

https://www.interviewbit.com/problems/valid-path/

There is a rectangle with left bottom as `(0, 0)` and right up as `(x, y)`.
There are `N circles` such that their centers are inside the `rectangle`.
Radius of each circle is `R`.
Now we need to find out if it is possible that we can move from `(0, 0)` to `(x, y)` without touching any circle.

**Note**: We can move from any cell to any of its 8 adjecent neighbours and we cannot move outside the boundary of the rectangle at any point of time.

### Input Format

```
1st argument given is an Integer x.
2nd argument given is an Integer y.
3rd argument given is an Integer N, number of circles.
4th argument given is an Integer R, radius of each circle.
5th argument given is an Array A of size N, where A[i] = x cordinate of ith circle
6th argument given is an Array B of size N, where B[i] = y cordinate of ith circle
```
### Output Format

Return YES or NO depending on weather it is possible to reach cell (x,y) or not starting from (0,0).

### Constraints

```
0 <= x, y, R <= 100
1 <= N <= 1000
Center of each circle would lie within the grid
```

### For Example

```
Input:
    x = 2
    y = 3
    N = 1
    R = 1
    A = [2]
    B = [3]
Output:
    NO
Explanation:
    There is NO valid path in this case
```
## Hint 1

Focus on every single points inside the rectangle. You can't go to some points which lie inside any of the circle. So basically you know all the points where you can stand at. Can you use this info to figure out a path between origin and destination.


## Solution Approach

Check if (i,j) is a valid point for all 0<=i<=x, 0<=j<=y. By valid point we mean that none of the circle should contain it.

Now you know all the valid point in rectangle. You need to figure out if you can go from (0,0) to (x,y) through valid points. This can be done with any graph traversal algorithms like BFS/DFS.

## Solution
```cpp
int X[] = { 0, 0, 1, 1, 1, -1, -1, -1 };
int Y[] = { 1, -1, 0, 1, -1, 0, 1, -1 };
string Solution::solve(int A, int B, int C, int D, vector<int> &E, vector<int> &F) {
    int rect[A + 1][B + 1];
    memset(rect, 0, sizeof(rect));
    for (int i = 0; i <= A; i++) {
        for (int j = 0; j <= B; j++) {
            for (int k = 0; k < C; k++) {
                if (sqrt(pow(E[k] - i, 2) + pow(F[k] - j, 2)) <= D) {
                    rect[i][j] = 1;
                    break;
                }
            }
        }
    }
    if (rect[0][0] == 1 || rect[A][B] == 1)
        return "NO";
    queue<pair<int, int>> q;
    q.push({ 0, 0 });
    rect[0][0] = 1;

    while (!q.empty()) {
        int x = q.front().first;
        int y = q.front().second;
        q.pop();

        if (x == A && y == B)
            return "YES";

        for (int i = 0; i < 8; i++) {
            int newx = x + X[i];
            int newy = y + Y[i];
            if (newx >= 0 && newx <= A && newy >= 0 && newy <= B && rect[newx][newy] == 0) {
                rect[newx][newy] = 1;
                q.push({ newx, newy });
            }
        }
    }
    return "NO";
}
```

## Asked in
* Morgan Stanley
* Amazon
* Codenation
