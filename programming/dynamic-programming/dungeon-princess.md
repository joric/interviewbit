# Dungeon Princess

https://www.interviewbit.com/problems/dungeon-princess/

The demons had captured the princess (P) and imprisoned her in the bottom-right corner of a dungeon. The dungeon consists of M x N rooms laid out in a 2D grid. Our valiant knight (K) was initially positioned in the top-left room and must fight his way through the dungeon to rescue the princess.

The knight has an initial health point represented by a positive integer. If at any point his health point drops to 0 or below, he dies immediately.

Some of the rooms are guarded by demons, so the knight loses health (negative integers) upon entering these rooms; other rooms are either empty (0's) or contain magic orbs that increase the knight's health (positive integers).

In order to reach the princess as quickly as possible, the knight decides to move only rightward or downward in each step.

Write a function to determine the knight's minimum initial health so that he is able to rescue the princess.

For example, given the dungeon below, the initial health of the knight must be at least 7 if he follows the optimal path

RIGHT-> RIGHT -> DOWN -> DOWN.

![Dungeon Princess: Example 1](http://i.imgur.com/5a6Neu4.png)

Input arguments to function:
Your function will get an M * N matrix (2-D array) as input which represents the 2D grid as described in the question. Your function should return an integer corresponding to the knight's minimum initial health required. 

### Note

The knight's health has no upper bound.

Any room can contain threats or power-ups, even the first room the knight enters and the bottom-right room where the princess is imprisoned.

## Hint 1

Start from bottom right (not from top left). 
Think Dynamic programming

Or think of the bruteforce solution and see if you can memoize.

## Solution Approach

There are only 2 positions you can directly go to from i, j. (i+1, j) and (i, j + 1). 
So if you knew the optimal path requirements for (i + 1, j) and (i, j + 1), you could choose the minimum of the two and be done with it.

## Solution

### Editorial
```cpp
int Solution::calculateMinimumHP(vector<vector<int>> &dungeon) {
    int M = dungeon.size();
    int N = dungeon[0].size();
    vector<vector<int>> hp(M + 1, vector<int>(N + 1, INT_MAX));
    hp[M][N - 1] = 1;
    hp[M - 1][N] = 1;
    for (int i = M - 1; i >= 0; i--) {
        for (int j = N - 1; j >= 0; j--) {
            int need = min(hp[i + 1][j], hp[i][j + 1]) - dungeon[i][j];
            hp[i][j] = need <= 0 ? 1: need;
        }
    }
    return hp[0][0];
}
```

### Fastest
```cpp
int Solution::calculateMinimumHP(vector<vector<int>> &A) {
    // Do not write main() function.
    // Do not read input, instead use the arguments to the function.
    // Do not print the output, instead return values as specified
    // Still have a doubt. Checkout www.interviewbit.com/pages/sample_codes/ for more details

    int m = A.size();
    int n = A[0].size();

    int hr[m][n]; // health required
    int ah[m][n]; // actual health

    hr[m - 1][n - 1] = 0;
    if (A[m - 1][n - 1] <= 0)
        hr[m - 1][n - 1] = (-1) * A[m - 1][n - 1];

    for (int j = n - 2; j >= 0; j--) {
        if (A[n - 1][j] <= 0)
            hr[n - 1][j] = hr[n - 1][j + 1] + (-1) * (A[n - 1][j]);
        else if (A[n - 1][j] < hr[n - 1][j + 1])
            hr[n - 1][j] = hr[n - 1][j + 1] - A[n - 1][j];
        else
            hr[n - 1][j] = 0;
        //cout<<hr[n-1][j]<<" ";
    }
    //cout<<endl;
    for (int i = n - 2; i >= 0; i--) {
        if (A[i][n - 1] <= 0)
            hr[i][n - 1] = hr[i + 1][n - 1] + (-1) * (A[i][n - 1]);
        else if (A[i][n - 1] < hr[i + 1][n - 1])
            hr[i][n - 1] = hr[i + 1][n - 1] - A[i][n - 1];
        else
            hr[i][n - 1] = 0;
        //cout<<hr[i][n-1]<<" ";
    }
    //cout<<endl;
    for (int i = n - 2; i >= 0; i--) {
        for (int j = n - 2; j >= 0; j--) {

            int req = min(hr[i + 1][j], hr[i][j + 1]);
            //cout<<i<<" "<<j<<" "<<req<<endl;
            if (A[i][j] <= 0)
                hr[i][j] = req + (-1) * (A[i][j]);
            else if (A[i][j] < req)
                hr[i][j] = req - A[i][j];
            else
                hr[i][j] = 0;
        }
    }
    return hr[0][0] + 1;
}

```

### Lightweight
```cpp
typedef long long       ll;
typedef vector<int>     vi;
typedef vector<vi>      vvi;
#define pb      push_back

int Solution::calculateMinimumHP(vector<vector<int> > &A) 
{
    int m = A.size();
	int n = A[0].size();
	A[m-1][n-1] = max(1-A[m-1][n-1], 1);	
	int i = m-1;
	for(int j = n-2; j >= 0; j--) {
		A[i][j] = max(A[i][j+1] - A[i][j], 1);
	}
	int j = n-1;
	for(int i = m-2; i >= 0; i--) {
		A[i][j] = max(A[i+1][j] - A[i][j], 1);
	}
	int data,x,y;
	for(int i = m-2; i >= 0; i--) {
		for(int j = n-2; j >= 0; j--) {
	       x = max(A[i][j+1] - A[i][j], 1);
	       y = max(A[i+1][j] - A[i][j], 1);
	       A[i][j] = min(x,y);
		}			
	}
	return A[0][0];
}
```

