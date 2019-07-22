# Flip Array

https://www.interviewbit.com/problems/flip-array/

Given an array of positive elements, you have to flip the sign of some of its elements such that the resultant sum of the elements of array should be minimum non-negative(as close to zero as possible). Return the minimum no. of elements whose sign needs to be flipped such that the resultant sum is minimum non-negative.
```
Constraints:

1 <= n <= 100

Sum of all the elements will not exceed 10,000.

Example:

A = [15, 10, 6]

ans = 1 (Here, we will flip the sign of 15 and the resultant sum will be 1 )

A = [14, 10, 4]

ans = 1 (Here, we will flip the sign of 14 and the resultant sum will be 0)
```

Note that flipping the sign of 10 and 4 also gives the resultant sum 0 but flippings there are not minimum 


## Hint 1

Given that we have to negate some of the elements such that total resultant sum should be minimum non-negative, can this problem be reduced to a knapsack problem?
Here, the elements of the knapsack would correspond to the elements negated.

Come on, think dynamic!

## Solution Approach

Let the sum of all the given elements be S. 

This problem can be reduced to a Knapsack problem where we have to fill a Knapsack of capacity (S/2) as fully as possible and using the minimum no. of elements. We will fill the Knapsack with the given elements. Sign of all the elements which come into the knapsack will be flipped.

As sum of all the elements in the Knapsack will be as close to S/2 as possible, we are indirectly calculating minimum non-negative sum of all the elements after flipping the sign. Give it a thought and code your way out!

## Solution

### Editorial
```cpp
// dp[i][j] = optimal solution for filling a knapsack of capacity j 
//using elements A[1,2,....,i]
//Optimal solution --> knapsack should be filled upto the capacity using least number of elements
struct node{
	int items;
	int weight;
}dp[105][10005];

int Solution::solve(const vector<int>& A){
	int n = A.size();
	int sum = 0;
	
	for(int i=0; i<n; i++) sum+=A[i];
	
	//knapsack algorithm for capacity sum/2
	int temp = sum/2;
	
	for(int i=0; i<=n; i++){
		for(int j=0; j<=temp; j++){
			if(i==0 || j==0) dp[i][j] = {0, 0};
			else{
				int prev_items = dp[i-1][j].items;
				int prev_weight = dp[i-1][j].weight;
				
				if(j-A[i-1] >= 0){
					int curr_weight = dp[i-1][j-A[i-1]].weight + A[i-1];
					int curr_items = dp[i-1][j-A[i-1]].items + 1;
					
					if((curr_weight>prev_weight) || ((curr_weight==prev_weight) && (curr_items<prev_items))){
						dp[i][j] = {curr_items, curr_weight};
					}
					else{
						dp[i][j] = dp[i-1][j];
					}
				} 
				else{
					dp[i][j] = dp[i-1][j];
				}
			}
		}
	}
	

	return dp[n][temp].items;
}
```

### Fastest
```cpp
int Solution::solve(const vector<int> &A) {
    int sum = 0;
    for (int i = 0; i < A.size(); i++)
        sum += A[i];
    int temp = sum / 2;
    vector<int> dp(temp + 1, INT_MAX);
    dp[0] = 0;
    for (int i = 0; i < A.size(); i++) {
        for (int j = temp; j >= A[i]; j--) {
            if (dp[j - A[i]] != INT_MAX) {
                dp[j] = min(dp[j], dp[j - A[i]] + 1);
            }
        }
    }
    for (int j = temp; j >= 0; j++) {
        if (dp[j] != INT_MAX)
            return dp[j];
    }
}

```

### Lightweight
```cpp
int Solution::solve(const vector<int> &A) {
    int totalSum = 0;
    for (int i = 0; i < A.size(); i++)
        totalSum += A[i];
    int items = A.size();
    vector<int> dummy(items + 1, INT_MAX);
    int nrows = totalSum / 2 + 1;
    vector<vector<int>> dp(nrows, dummy);
    for (int i = 0; i < nrows; i++) {
        for (int j = 0; j <= items; j++) {

            if (i == 0)
                dp[i][j] = 0;
            else if (j == 0 && i != 0)
                dp[i][j] = INT_MAX;
            else {
                int include = INT_MAX, exclude = INT_MAX;
                if (i - A[j - 1] >= 0 && dp[i - A[j - 1]][j - 1] != INT_MAX)
                    include = 1 + dp[i - A[j - 1]][j - 1];
                exclude = dp[i][j - 1];
                dp[i][j] = min(include, exclude);
            }
        }
    }
    for (int i = nrows - 1; i >= 0; i--) {
        if (dp[i][items] != INT_MAX) {
            return dp[i][items];
        }
    }
}

```

### Mine
```cpp
int Solution::solve(const vector<int> &A) {
    int n = A.size();
    int sum = accumulate(A.begin(), A.end(), 0);
    vector<vector<pair<int, int>>> dp((sum / 2) + 1, vector<pair<int, int>>(n + 1, { 0, 0 }));
    for (int i = 1; i <= sum / 2; i++) {
        for (int j = 1; j <= n; j++) {
            if (A[j - 1] > i) {
                dp[i][j] = dp[i][j - 1];
            } else {
                pair<int, int> incl{ dp[i - A[j - 1]][j - 1].first + A[j - 1], 1 + dp[i - A[j - 1]][j - 1].second };
                pair<int, int> excl{ dp[i][j - 1].first, dp[i][j - 1].second };
                auto comp = [](const pair<int, int> &a, const pair<int, int> &b) {
                    if (a.first == b.first) {
                        return a.second < b.second;
                    }
                    return a.first > b.first;
                };

                dp[i][j] = min(incl, excl, comp);
            }
        }
    }
    return dp[sum / 2][n].second;
}
```

## Asked in
