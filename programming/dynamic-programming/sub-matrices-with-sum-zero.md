# Sub Matrices with sum Zero

https://www.interviewbit.com/problems/sub-matrices-with-sum-zero/

Given a 2D matrix, find the number non-empty sub matrices, such that the sum of the elements inside the sub matrix is equal to 0. (note: elements might be negative).

### Example

```
Input

-8 5  7
3  7 -8
5 -8  9
Output
2

Explanation
-8 5 7
3 7 -8
5 -8 9

-8 5 7
3 7 -8
5 -8 9
```

## Hint 1

Try to convert this problem to 1D version.

## Hint 2

To convert to 1D, try to compress the columns between 2 fixed rows and then to solve the 1D version on the compressed array.

## Solution Approach

Iterate over all pairs of rows. When fixing two rows r1 and r2, we can convert this to 1D version of the problem.

When we have a 1D array ARR we want to find number of subarrays such that the sum of the elements in the subarray is equal to 0. To do that lets iterate from left to right, say we are currently at i-th element. If we have i-th prefix sum equal to sum(ARR[0..i]), then we want to find number of such j’s that sum(ARR[0..i]) = sum(ARR[0..j]). That means that the subarray ARR[j + 1..i] will have zero sum. To efficiently count number of such j’s we can use a HashMap (unordered_map in C++).

In order to convert the problem to 1D, when we have a pair of fixed rows r1 and r2, we will keep a 2D prefix sums, let’s call it PRE (let’s also assume that initial matrix is A). PRE[i, j] will be the sum of elements in sub matrix whose upper left corner is [0, 0] and lower right corner is [i, j]. In other words it is a sum of all A[p, q] where 0 <= p <= i and 0 <= q <= j. 

The calculation of PRE is very easy: PRE[i, j] = A[i, j] + PRE[i - 1, j] + PRE[i, j - 1] - PRE[i - 1, j - 1] (if i - 1 or j - 1 are less than 0 then we just omit the terms where they appear). Notice, that we need to subtract PRE[i - 1, j - 1] since it is contained in both PRE[i - 1, j] and PRE[i, j - 1] and we want every element to appear in PRE[i, j] exactly once. This is called inclusion exclusion principle.

When we have two fixed rows r1, r2 and have calculated PRE, we can obtain ARR. Note that we don’t really need to calculate each element of ARR, since we only need prefix sums of ARR, that is sum(ARR[0..i]) for each i. The sum(ARR[0..i]) is equal to PRE[r2][i] - PRE[r1 - 1][i] (if r1 - 1 < 0 then omit second operand). Being able to efficiently calculate sum(ARR[0..i]), let’s apply the 1D solution.

The answer to the problem will be simply the sum of answers for all different pairs of rows.

Overall time complexity is O(N3).
Space complexity is O(N2)

## Solution

### Editorial
```cpp
int fn(vector<int> &arr){
    int n=arr.size();
    unordered_map<int, int> prevSum; 
    int res = 0;  
    int currsum = 0; 
    for (int i = 0; i < n; i++) { 
        currsum += arr[i]; 
        if (currsum == 0)  
            res++;         
        if (prevSum.find(currsum) != prevSum.end())  
            res += (prevSum[currsum]); 
        prevSum[currsum]++; 
    } 
    // for (int i = 0; i < n; i++) cout<<arr[i]<<" ";
    // cout<<res<<" ";
    return res;
}
int Solution::solve(vector<vector<int> > &A) {
    if(A.size()==0)return 0;
    int m=A.size(),n=A[0].size();
    int ans=0;
    for(int i=0;i<m;i++){
        vector<int> t(n,0);
        //ans+=fn(t);
        for(int j=i;j<m;j++){
            for(int k=0;k<n;k++){
                t[k]+=A[j][k];
            }
            ans+=fn(t);
        }
    }
    return ans;
}
```

### Fastest
```cpp
int Solution::solve(vector<vector<int> > &A) 
{
    if(A.size()==0 || A[0].size()==0) return 0;
    int dp[A.size()+1][A[0].size()+1];
    int n=A.size()+1,m=A[0].size()+1;
    for(int i=0;i<n;i++) 
        for(int j=0;j<m;j++) 
            dp[i][j]=0;
    for(int i=1;i<n;i++) 
        for(int j=1;j<m;j++) {
            dp[i][j]+=dp[i-1][j]+dp[i][j-1]-dp[i-1][j-1];
            dp[i][j]+=A[i-1][j-1];
            //cout<<i<<' '<<j<<' '<<dp[i][j]<<"  ";
        }
    int ans=0;
    for(int i=0;i<n;i++)
     for(int j=i+1;j<n;j++) {
         int a[m];
         for(int k=0;k<m;k++) 
          {a[k] = dp[j][k]-dp[i][k];
           //cout<<a[k]<<' ';
          }
          //cout<<' ';
         sort(a,a+m);
         int c=1;
         for(int k=1;k<m;k++)
          if(a[k]==a[k-1])
           c++;
          else {
              ans+=(c*(c-1))/2;
              c=1;
          }
          ans+=(c*(c-1))/2;
          //cout<<ans<<' ';
     }
     return ans;
}

```

### Lightweight
```cpp
int Solution::solve(vector<vector<int>> &A) {

    int m = A.size(), n, j, i, ans = 0, k, l;
    if (m != 0)
        n = A[0].size();
    else
        return 0;

    for (i = 0; i < m; i++) {
        for (j = 0; j < n; j++) {
            if (i == 0 && j == 0) {
                continue;
            }
            if (i == 0)
                A[0][j] += A[0][j - 1];
            else if (j == 0)
                A[i][0] += A[i - 1][0];

            else
                A[i][j] += A[i][j - 1] + A[i - 1][j] - A[i - 1][j - 1];
        }
    }

    for (i = 0; i < m; i++) {
        for (j = 0; j < n; j++) {
            for (k = i; k < m; k++) {

                for (l = j; l < n; l++) {
                    int sub = A[k][l];
                    if (i > 0)
                        sub -= A[i - 1][l];
                    if (j > 0)
                        sub -= A[k][j - 1];
                    if (i > 0 && j > 0)
                        sub += A[i - 1][j - 1];

                    if (sub == 0) {

                        ans++;
                    }
                }
            }
        }
    }

    return ans;
}
```

### Mine
```cpp
int Solution::solve(vector<vector<int> > &A) {
    const vector<vector<int>>& mat = A;
    
    int r = mat.size();
    
    if(r == 0) {
        return 0;
    }
    
    int c = mat[0].size();
    
    if(c == 0) {
        return 0;
    }
    
    vector<vector<int>> sum(r + 1, vector<int>(c + 1, 0));

    for(int i = 0; i < r; i++) {
        for(int j = 0; j < c; j++) {
            sum[i + 1][j + 1] = mat[i][j] + sum[i + 1][j] + sum[i][j + 1] - sum[i][j];
        }
    }
    
    unordered_map<int, vector<int>> seen;
    int count = 0;

    for(int i = 0; i < r; i++) {
        for(int j = i + 1; j <= r; j++) {
            seen.clear();
            seen[0].push_back(0);
            int cur_count = 0;
            for(int k = 1; k <= c; k++) {
                int diff = sum[j][k] - sum[i][k];
                seen[diff].push_back(k);
            }
            
            for(auto& d: seen) {
                int d_count = d.second.size();
                count = count + (d_count * (d_count - 1)) / 2;
            }
        }
    }

    return count;
}

```

## Asked in

* Google
