# Minimum operations of given type to make all elements of a matrix equal

https://www.interviewbit.com/problems/minimum-operations-of-given-type-to-make-all-elements-of-a-matrix-equal/


Given a matrix of integers A of size N x M and an integer B.

In a single operation, B can be added to or subtracted from any element of the matrix.

Find and return the minimum number of operations required to make all the elements of the matrix equal and 
if it impossible return -1 instead.

Note: Rows are numbered from top to bottom and columns are numbered from left to right.

### Input Format

The first argument given is the integer matrix A.
The second argument given is the integer B.

### Output Format

Return the minimum number of operations required to make all the elements of the matrix equal and if it impossible return -1 instead.

### Constraints
```
1 <= N, M <= 1000
-1000 <= A[i] <= 1000
1 <= B <= 1000
```
### For Example
```
Input 1:
    A = [   [0, 2, 8]
            [8, 2, 0]
            [0, 2, 8]   ]
    B = 2
Output 1:
    12

Input 2:
    A = [   [5, 17, 100, 11]
            [0, 0,  2,   8]    ]
    B = 3
Output 2:
    -1
```

## Solution

### Editorial (fails)
```cpp
int Solution::solve(vector<vector<int> > &a, int b) {
    int n = a.size(), m = a[0].size();
    long d[n*m];
    int mod = a[0][0] % b;
    for(int i = 0; i < n; i++) {
        for(int j = 0; j < m; j++) {
            if(a[i][j] % b != mod) {
                return -1;
            }   d[i*m + j] = a[i][j];
        }
    }
    sort(d, d + n*m);
    int mid = (n * m) >> 1;
    long ans = 0;
    for(long x : d) ans += abs(x - d[mid]);
    if(n*m % 2 == 0) {
        mid--;
        long ans2 = 0;
        for(long x : d) ans2 += abs(x - d[mid]);
        ans = min(ans, ans2);
    }
    return (int)ans;
}
```

### Fastest (passes)
```cpp
int Solution::solve(vector<vector<int> > &A, int B) {
    int min_value=INT_MAX, max_value=INT_MIN;
    for(vector<int> v: A)
    {
        auto itr = min_element(v.begin(), v.end());
        if(*itr<min_value)
        {
            min_value = *itr;
        }
        itr = max_element(v.begin(), v.end());
        if(*itr>max_value)
        {
            max_value = *itr;
        }
    }
    
    int min_ops = INT_MAX;
    
    for(int target = min_value; target<=max_value; target++)
    {
        bool possible = true;
        int ops = 0;
        for(int i = 0; i<A.size() && possible; i++)
        {
            for(int j = 0; j<A[0].size() && possible; j++)
            {
                int diff = abs(target-A[i][j]);
                if(diff%B!=0)
                {
                    possible = false;
                }
                else
                {
                    ops+=diff/B;
                }
            }
        }
        
        if(possible)
        {
            if(ops<min_ops)
            {
                min_ops = ops;
            }
        }
    }
    
    if(min_ops==INT_MAX) return -1;
    return min_ops;
}
```

### Lightweigth
```cpp
int getCount(vector<vector<int> > &A,int t,int B){
    int n=A.size();
    int m=A[0].size();
    int ans=0;
    for(int i=0;i<n;i++){
        for(int j=0;j<m;j++){
            ans+=(abs(t*B-A[i][j])/B);
        }
    }
    return ans;
}
int Solution::solve(vector<vector<int> > &A, int B) {
    int n=A.size();
    int m=A[0].size();
    int rem=(A[0][0]+1005)%B;
    int mina=INT_MAX;
    int maxa=-1;
    for(int i=0;i<n;i++){
        for(int j=0;j<m;j++){
            A[i][j]+=1005;
            if(A[i][j]%B!=rem)
                return -1;
            A[i][j]-=rem;
            mina=min(A[i][j],mina);
            maxa=max(A[i][j],maxa);
        }
    }
    int l=mina/B,u=maxa/B;
        while(u-l>1){
            int mid=(u+l)/2;
            int ct1=getCount(A,mid-1,B);
            int ct2=getCount(A,mid,B);
            int ct3=getCount(A,mid+1,B);
            if(ct2<=ct1 && ct2<=ct3){
                return ct2;
            }
            else if(ct1>ct2 && ct2>ct3){
                l=mid;
            }
            else if(ct1<ct2 && ct2<ct3){
                u=mid;
            }
            else{
                cout<<"equals error"<<endl;
                return -1;
            }
        }
        return min(getCount(A,l,B),getCount(A,u,B));
    
    
}
```
### Mine (fails tests)
```cpp
int minOperations(int n, int m, int k, vector<vector<int> >& matrix) { 
    // Create another array to 
    // store the elements of matrix 
    vector<int> arr(n * m, 0); 
  
    int mod = matrix[0][0] % k; 
  
    for (int i = 0; i < n; ++i) { 
        for (int j = 0; j < m; ++j) { 
            arr[i * m + j] = matrix[i][j]; 
  
            // If not possible 
            if (matrix[i][j] % k != mod) { 
                return -1; 
            } 
        } 
    } 
  
    // Sort the array to get median 
    sort(arr.begin(), arr.end()); 
  
    int median = arr[(n * m) / 2]; 
  
    // To count the minimum operations 
    int minOperations = 0; 
    for (int i = 0; i < n * m; ++i)  
        minOperations += abs(arr[i] - median) / k; 
  
    // If there are even elements, then there  
    // are two medians. We consider the best 
    // of two as answer. 
    if ((n * m) % 2 == 0) 
    { 
       int median2 = arr[(n * m) / 2]; 
       int minOperations2 = 0; 
       for (int i = 0; i < n * m; ++i)  
          minOperations2 += abs(arr[i] - median2) / k; 
  
       minOperations = min(minOperations, minOperations2); 
    } 
  
    // Return minimum operations required 
    return minOperations; 
} 

int Solution::solve(vector<vector<int> > &matrix, int k) {
    int n = matrix.size();
    int m = matrix[0].size(); 
    return minOperations(n, m, k, matrix);
}
```
