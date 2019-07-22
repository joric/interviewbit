# Ways to color a 3xN Board

https://www.interviewbit.com/problems/ways-to-color-a-3xn-board/

Given a 3Xn board, find the number of ways to color it using at most 4 colors such that no two adjacent boxes have same color. Diagonal neighbors are not treated as adjacent boxes. 
Output the ways%1000000007 as the answer grows quickly.

1<= n < 100000
```
Example:
Input: n = 1
Output: 36
```

## Hint 1

Suppose that you filled a given column in 3Xn board with a triplet of colors. Which other triplets can you use to fill the next column?

Think DP.

## Solution Approach

Let us first color a given column with three colors such that no two adjacent boxes have the same color. A simple combinatorics reveal that there are 36 ways to do so. 

Let's say colors are: {0,1,2,3}

Triplets of color: {0,1,2} , {3,1,2}, {4,1,2}, .....

Now, suppose that we color the given column with one of these triplets. We acan color the next column with the triplets of color that do not contradict our coloring scheme.

E.g.: | 0 | 1 | - | - |
|||||
 -    | 1 | 3 | - | - |
 -    | 2 | 0 | - | - |

Here, we can color third column using triplets that go well with the triplet {1,3,0}. These are {0,1,2}, {2,1,3}, ......

This can be coded using a simple dynamic programming approach.

Recurrence: 

{i,j,k} = triplet of color used to paint nth column in the order given. (1st row: i, 2nd row: j, .... }

solve(i,j,k,n) = no. of ways to color a 3xn board such that nth column is painted with color triplet {i,j,k}

solve(i,j,k,n) =$\sum_{triplets(x,y,z)}$ ( solve(x,y,z, n-1) ) such that {i,j,k} and {x,y,z} go well with each other.


## Solution

### Editorial
```cpp
#define mod 1000000007
#define ll long long

struct triplet{
	int x, y, z;
};

vector<triplet> trip;  //vector of 36 triplets

void fillTriplets(){
	//trip vector contain the triplets of color that can be used to paint a certain column
       trip.clear();
	for(int i=0; i<4; i++){
		for(int j=0; j<4; j++){
			for(int k=0; k<4; k++){
				if(i!=j && j!=k) trip.push_back({i,j,k});
			}
		}
	}
}

int dp[4][4][4][100005];


bool isCompatible(const triplet& t1,  const triplet& t2){
	//check if triplets t1 and t2 are compatible
	if(t1.x==t2.x || t1.y==t2.y || t1.z==t2.z ){
		return 0;
	}
	return 1;
}

int Solution::solve(int n){
        fillTriplets();
	if(n<=0) return -1;
	
       //Bottom-up dp
	for(int i=1; i<=n; i++){
		for(int j=0; j<36; j++){
			if(i==1) dp[trip[j].x][trip[j].y][trip[j].z][i] = 1;
			else{
				ll temp = 0;
				
				for(int k=0; k<36; k++){
					if(isCompatible(trip[j], trip[k])){
						temp += dp[trip[k].x][trip[k].y][trip[k].z][i-1];
						temp %= mod;
					}
				}
				dp[trip[j].x][trip[j].y][trip[j].z][i] = temp;
			}
		}
	}
	
	ll ans = 0;
	for(int i=0; i<trip.size(); i++){
		ans = (ans + dp[trip[i].x][trip[i].y][trip[i].z][n])%mod;
	}
         int res = ans;

	return ans;
}

```

### Mine
```cpp
int Solution::solve(int A) {
    long int color3 = 24;
    long int color2 = 12;
    long int temp = 0;
    for (int i = 2; i <= A; i++) {
        temp = color3;
        color3 = (11 * color3 + 10 * color2) % 1000000007;
        color2 = (5 * temp + 7 * color2) % 1000000007;
    }
    return (color3 + color2) % 1000000007;
}
```

## Asked in
* Codenation
