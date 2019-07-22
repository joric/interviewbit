# Max Rectangle in Binary Matrix

https://www.interviewbit.com/problems/max-rectangle-in-binary-matrix/

Given a 2D binary matrix filled with 0's and 1's, find the largest rectangle containing all ones and return its area.

Bonus if you can solve it in O(n^2) or less.

Example :
```
A: [
1 1 1
0 1 1
1 0 0
]

Output: 4 

As the max area rectangle is created by the 2x2 rectangle created by (0,1), (0,2), (1,1) and (1,2)
```

## Hint 1

The bruteforce approach is to look at all pairs of (i,j) to (k,l) and check if its filled with 1s. This approach however is O(NNNNN^2) = O(N^6). [ N^4 ways to choose i,j,k,l and then N^2 elements in the square ]. 
Can you optimize this approach if you had additional space to store results for your previous calculations ? 
Maybe if you knew the result for (i, j) to (k, l - 1) or (i, j) to (k - 1, l) or both ?

## Hint 2

We can improve from N^6 by storing in dp[i][j][k][l] if (i,j) to (k,l) is all filled with 1. 
dp[i][j[k][l] = 1 iff dp[i][j][k][l-1] = 1 && dp[i][j][k-1][l] = 1 and matrix[k][l] = 1.

Now we can improve this further. What if with every (i,j) we stored the length of 1s in the same row i starting from (i,j). 
Can we move down in the column j from row i and determine the largest rectangle without having to visit all cells ?

## Solution Approach

Lets max_x[i][j] denote the length of 1s in the same row i starting from (i,j).

So our current max with one end of the rectangle at (i,j) would be max_x[i][j]. 
As we move to the next row, there are 2 cases: 
1) max_x[i+1][j] >= max_x[i][j] which means that we can take max_x[i][j] 1s from next column as well and extend our current rectangle as it is, with one more extra row.
```
11100000 - 111
11111100 - 111
```

2) max_x[i+1][j] < max_x[i][j] which means that if we want to extend our current rectangle to next row, we need to reduce the number of columns in it to max_x[i+1][j]
```
11100000 - 11 
11000000 - 11
```
As mentioned above, we keep increasing the columns and adjusting the width of the rectangle. 
O(N^3) time complexity.

Even though N^3 is acceptable, it might be worth exploring a better solution. 
If you notice, laying out max_x[i][j] helps you make histograms in every row. Then the problem becomes of finding the maximum area in histograms ( which we have solved before in Stacks and Queues ) in O(n). This would lead to an O(N^2) solution. We strongly suggest you to explore the O(N^2) solution as well.

## Solution

### Editorial
```cpp
int Solution::maximalRectangle(vector<vector<int>> &matrix) {
    int rows = matrix.size();
    if (rows == 0) return 0;
    int cols = matrix[0].size();
    if (cols == 0) return 0;
    vector<vector<int>> max_x(rows, vector<int>(cols, 0));
    int area = 0;
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            if (matrix[i][j] == 1) {
                if (j == 0)
                    max_x[i][j] = 1;
                else
                    max_x[i][j] = max_x[i][j - 1] + 1;
                int y = 1;
                int x = cols;
                while ((i - y + 1 >= 0) && (matrix[i - y + 1][j] == 1)) {
                    x = min(x, max_x[i - y + 1][j]);
                    area = max(area, x * y);
                    y++;
                }
            }
        }
    }
    return area;
}
```

### Mine
```cpp
int Solution::maximalRectangle(vector<vector<int> > &A) {
    // Do not write main() function.
    // Do not read input, instead use the arguments to the function.
    // Do not print the output, instead return values as specified
    // Still have a doubt. Checkout www.interviewbit.com/pages/sample_codes/ for more details
    
    if(A.size() == 0){
        return 0;
    }
    
    vector<int> temp(A[0].size(), 0);
    
    int i = 0;
    
    int sol = 0;
    
    while(i < A.size()){
        
        for(int k = 0; k < A[i].size(); k++){
            if(A[i][k] == 0){
                temp[k] = 0;
            }
            else{
                temp[k] = temp[k] + A[i][k];
            }
        }
        
        stack<int> st;
        
        int j = 0;
        
        while(j < temp.size()){
            if(st.empty() || temp[st.top()] <= temp[j]){
                st.push(j);
                j++;
            }
            else{
                int top = st.top();
                st.pop();
                
                int area = 0;
                
                if(st.empty()){
                    area = temp[top] * j;
                }
                else{
                    area = temp[top] * (j - st.top() - 1);
                }
                
                sol = max(sol, area);
            }
        }
        
        while(!st.empty()){
            int top = st.top();
            st.pop();
            
            int area = 0;
            
            if(st.empty()){
                area = temp[top] * j;
            }
            else{
                area = temp[top] * (j - st.top() - 1);
            }
            
            sol = max(sol, area);
        }
    
        i++;    
    }
    
    return sol;
}

```

## Asked in
* Google
* Microsoft
