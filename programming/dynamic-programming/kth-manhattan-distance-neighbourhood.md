# Kth Manhattan Distance Neighbourhood

Given a matrix M of size nxm and an integer K, find the maximum element in the K manhattan distance neighbourhood for all elements in nxm matrix. 
In other words, for every element M[i][j] find the maximum element M[p][q] such that abs(i-p)+abs(j-q) <= K.

Note: Expected time complexity is O(N*N*K)

Constraints:
```
1 <= n <= 300
1 <= m <= 300
1 <= K <= 300
0 <= M[i][j] <= 1000
```
Example:
```
Input:
M  = [[1,2,4],[4,5,8]] , K = 2

Output:
ans = [[5,8,8],[8,8,8]]
```

## Hint 1

If you have answer for (K-1)th manhtaan distance of all the elements, can you use those values to find the answer for Kth manhattan distance.

Come on, think dynamic !

## Solution Approach

This problem can be solved easily using dynamic programming.

DP recurrence:
```
dp[k][i][j] = ans. for kth manhattan distance for element (i,j)

dp[k+1][i][j] = max(dp[k][i-1][j], dp[k][i+1][j], dp[k][i][j-1], dp[k][i][j+1], dp[k][i][j] )
```
Recurrence is easy to get once you draw the figure.


## Solution

### Editorial
```cpp
bool check(int i, int j, int n, int m){
	//Check if (i,j) are with in matrix dimensions
	if(i >= 0 && j >=0 && i < n && j < m) return 1;
	return 0;
}

vector<vector<int> > Solution::solve(int A, vector<vector<int> > &B) {
	// DP can be optimized to be of N*N size as we need only dp[k-1][n][n] for dp[k][n][n]
	int n = B.size();
	int m = B[0].size();
	
	vector<vector<vector<int> > > dp(2);
	dp[0] = dp[1] = B;
	
	int rplus[4] = {1,0,0,-1};
	int cplus[4] = {0,-1,1,0};
	
	for(int k=0; k <= A; k++){
		for(int i=0; i<n; i++){
			for(int j=0; j<m; j++){
				//base case
				if( k== 0) dp[k][i][j] = B[i][j];
				//dp[k][i][j] = max(dp[k-1][i-1][j],dp[k-1][i+1][j],dp[k-1][i][j-1],
				 //                                   dp[k-1][i][j+1],dp[k-1][i][j])
				else{
					int ans = dp[(k-1)%2][i][j];
					for(int p = 0; p < 4; p++){
						int temp_i = i+rplus[p];
						int temp_j = j+cplus[p];
						if(check(temp_i, temp_j, n, m)) ans = max(ans, dp[(k-1)%2][temp_i][temp_j]);
					}
					dp[k%2][i][j] = ans;
	
				}
			}
		}
	}
	
	return dp[A%2];
}
```

### Mine
```cpp
int max_of(int a, int b, int c, int d, int e) {
    return max(max(max(a, b), max(c, d)), e);
}

// Time - O(N * K), Space - O(N)
// where N = size of 2d array, K = distance given
vector<vector<int> > Solution::solve(int A, vector<vector<int> > &B) {
    int rows = B.size();
    int cols = B[0].size();

    int max_distance = A;
    vector<vector<int>> cur_distances = B;
    vector<vector<int>> next_distances = cur_distances;

    for (int dist = 0; dist < max_distance; dist++) {
        for (int r = 0; r < rows; r++) {
            for (int c = 0; c < cols; c++) {

                int cur = cur_distances[r][c];
                int right = (c == cols - 1) ? -1 : cur_distances[r][c + 1];
                int up = (r == 0) ? -1 : cur_distances[r - 1][c];
                int left = (c == 0) ? -1 : cur_distances[r][c - 1];
                int down = (r == rows - 1) ? -1 : cur_distances[r + 1][c];

                next_distances[r][c] = max_of(cur, right, up, left, down);
            }
        }

        cur_distances = next_distances;
    }

    return cur_distances;
}
```

## Asked in
* Liv.ai