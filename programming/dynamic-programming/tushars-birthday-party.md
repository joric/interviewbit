# Tushar's Birthday Party

https://www.interviewbit.com/problems/tushars-birthday-party/

As it is Tushar's Birthday on March 1st, he decided to throw a party to all his friends at TGI Fridays in Pune.
Given are the eating capacity of each friend, filling capacity of each dish and cost of each dish. A friend is satisfied if the sum of the filling capacity of dishes he ate is equal to his capacity. Find the minimum cost such that all of Tushar's friends are satisfied (reached their eating capacity).

NOTE:

Each dish is supposed to be eaten by only one person. Sharing is not allowed.
Each friend can take any dish unlimited number of times.
There always exists a dish with filling capacity 1 so that a solution always exists.

```
Input Format

Friends: List of integers denoting eating capacity of friends separated by space.
Capacity: List of integers denoting filling capacity of each type of dish.
Cost:    List of integers denoting cost of each type of dish.
Constraints:
1 <= Capacity of friend <= 1000
1 <= No. of friends <= 1000
1 <= No. of dishes <= 1000

Example:

Input:
    2 4 6
    2 1 3
    2 5 3

Output:
    14
```
Explanation: 
    First friend will take 1st and 2nd dish, second friend will take 2nd dish twice.  Thus, total cost = (5+3)+(3*2)= 14

## Hint 1

As friends cannot share dishes, can we independently calculate the cost for each friend and add them up?
If we calculate answer for each person independently, how different is this from the standard Knapsack problem?

Time for some Dynamic thinking!

## Solution Approach

**Observations: **

As the friends cannot share dishes, we can calculate the cost for each of them independently and add all such costs.
Now, the problem instance for every friend is reduced to standard KnapSack problem.

**Dynamic programming recurrence: **

```
dp[i][j] -> min. cost to satisfy a person with capacity i using first j dishes.
dp[i][j] = min( dp[i][j-1] , dp[ i-fillCap[j] ][j] + cost[j] ) // if ( i-fillCap[j] ) >= 0
dp[i][j] = dp[i][j-1] // otherwise
```

As one dish can be taken multiple times, we have used dp[ i-fillCap[j] ][ j ] and not dp[ i-fillCap[j] ][ j-1 ]. This is different from standard KnapSack where one element can be used only once.

Note: Base cases should be handled properly.

## Solution

### Editorial
```cpp
#define INF 10000000

int dp[1005][1005];

int Solution::solve(const vector<int> &A,  const vector<int> &B, const vector<int> &C){
	// vector of pair {dish_items, dish_cost}
	vector<pair<int,int> > dish;
	int n = C.size();	
	for(int i=0; i<n; i++)	dish.push_back(make_pair(B[i],C[i]));
	
	//maximum capacity among friends
	int m = -1;
	for(int i=0; i<A.size(); i++){
		m = max(m, A[i]);
	}
	
	// dp[highest capacity][no. of dishes]
	for(int i=0; i<=m; i++){
		for(int j = 0; j<=n; j++){
			//if capacity of friend is 0
			if(i == 0) dp[i][j] = 0;
			//if no dish is remaining to choose from
			else if(j==0) dp[i][j] = INF;
			else{
				//if i-th person can eat jth dish
				if(i >= dish[j-1].first){
					//As one dish can be taken multiple times, we have used 
					//dp[ i-dish[j-1].first ][ j ] and not dp[ i-dish[j-1].first ][ j-1 ]. 
					
					dp[i][j] = min(dp[i][j-1], dp[i-dish[j-1].first][j] + dish[j-1].second);
				}
				else dp[i][j] = dp[i][j-1];
			} 
		}	
	}
	
	// Add for each friend independently
	int ans=0;
	for(int i=0; i<A.size(); i++){
		ans += dp[A[i]][n];
	}
	
	return ans;
}
```

### Fastest
```cpp
int getMax(const vector<int> &arr) {
    int ans = 0;
    for(int i=0;i<arr.size();i++) {
        ans = max(ans, arr[i]);
    }
    return ans;
}
int Solution::solve(const vector<int> &friends, const vector<int> &dishes, const vector<int> &cost) {
    int capacity = getMax(friends);
    vector<int> minCost(capacity+1, 1e9);
    minCost[0] = 0;
    
    for(int i=0;i<dishes.size();i++) {
        for(int j= dishes[i];j<=capacity;j++) {
            if (j >= dishes[i]) {
                minCost[j] = min(minCost[j], minCost[j-dishes[i]] + cost[i]);
            }
        }
    }
    
    int ans = 0;
    for(int afriend: friends) {
        ans += minCost[afriend];
    }
    return ans;
    
}
```

### Lightweight
```cpp
int Solution::solve(const vector<int> &A, const vector<int> &B, const vector<int> &C) {
    int cst[10000];
    memset(cst,-1,sizeof cst);
   
    cst[0]=0;
    int l = B.size();
    for(int i = 0; i < l; i++){
        for(int j = 1; j <=1000; j++){
            if(j-B[i]>=0 && cst[j-B[i]] != -1){
                if(cst[j]==-1){
                    cst[j] = cst[j-B[i]] + C[i];
                }else
                cst[j]=min(cst[j],cst[j-B[i]] + C[i]);
            }
        }
    }
    int ans = 0;
    for(int a: A){
        ans += cst[a];
    }
    return ans;
}

```

### Mine
```cpp
#define MOD 1000000007

int Solution::solve(const vector<int> &A, const vector<int> &B, const vector<int> &C) {
    const vector<int> &friends_cap = A;
    const vector<int> &dishes_fill = B;
    const vector<int> &dishes_cost = C;

    int max_cap = *max_element(friends_cap.begin(), friends_cap.end());
    vector<vector<int>> dp(max_cap + 1, vector<int>(dishes_fill.size() + 1, 0));

    for (int cap = 1; cap <= max_cap; cap++) {
        dp[cap][0] = MOD;
    }

    for (int cap = 1; cap <= max_cap; cap++) {
        for (int dish = 1; dish <= dishes_fill.size(); dish++) {
            if (cap - dishes_fill[dish - 1] >= 0) {
                int cur_dish_fill = dishes_fill[dish - 1];
                int cur_dish_cost = dishes_cost[dish - 1];
                dp[cap][dish] =
                    min(dp[cap][dish - 1],
                        dp[cap - cur_dish_fill][dish] + cur_dish_cost);
            } else {
                dp[cap][dish] = dp[cap][dish - 1];
            }
        }
    }

    int result = 0;
    for (const auto &cap: friends_cap) {
        result += dp[cap][dishes_fill.size()];
    }

    return result;
}

```

## Asked in
* Snapdeal
