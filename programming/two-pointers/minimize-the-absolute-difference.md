# Minimize The Absolute Difference

https://www.interviewbit.com/problems/minimize-the-absolute-difference


	Minimize the absolute difference
	Given three sorted arrays A, B and Cof not necessarily same sizes.
	Calculate the minimum absolute difference between the maximum and minimum number from the triplet a, b, c such that a, b, c belongs arrays A, B, C respectively.
	i.e. minimize | max(a,b,c) - min(a,b,c) |.
## Solution

```cpp

int Solution::solve(vector<int> &A, vector<int> &B, vector<int> &C) {
    int ans = INT_MAX;
    int i=A.size()-1,j=B.size()-1,k=C.size()-1;
    while(i>=0 and j>=0 and k>=0) {
        int maxe = max(A[i], max(B[j],C[k]));
        int mine = min(A[i], min(B[j],C[k]));
        ans = min(ans, maxe - mine);
        if(A[i]==maxe) i--;
        else if(B[j]==maxe) j--;
        else if(C[k]==maxe) k--;
    }
    return ans;
}


/* my solution */
int Solution::solve(vector<int> &A, vector<int> &B, vector<int> &C) {
    
        int min_diff, current_diff, max_term; 
        
        int i=A.size()-1;
        int j=B.size()-1;
        int k=C.size()-1;
  
        // calculating min difference from last 
        // index of lists 
        min_diff = abs(max(A[i], max(B[j], C[k]))  
                     - min(A[i], min(B[j], C[k]))); 
  
        while (i != -1 && j != -1 && k != -1)  
        { 
            current_diff = abs(max(A[i], max(B[j], C[k]))  
                            - min(A[i], min(B[j], C[k]))); 
  
            // checking condition 
            if (current_diff < min_diff) 
                min_diff = current_diff; 
  
            // calculating max term from list 
            max_term = max(A[i], max(B[j], C[k])); 
  
            // Moving to smaller value in the 
            // array with maximum out of three. 
            if (A[i] == max_term) 
                i -= 1; 
            else if (B[j] == max_term) 
                j -= 1; 
            else
                k -= 1; 
        } 
          
        return min_diff; 
        
}

```