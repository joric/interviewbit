# Matrix permutation

https://www.interviewbit.com/problems/matrix-permutation/

Given a matrix of integers A of size N x N.Find whether all rows are circular rotations of each other or not.

Return "YES",if all rows of A are circular rotations of each other, else return "NO".

Note: Rows are numbered from top to bottom and columns are numbered from left to right.

### Input Format

The first argument given is the integer matrix A.

### Output Format

Return "YES",if all rows of A are circular rotations of each other, else return "NO".

### Constraints
```
1 <= N <= 1000
1 <= A[i] <= 100000
```
### For Example
```
Input 1:
    A = [   [1, 2, 3]
            [3, 2, 1]
            [2, 3, 1]   ]
Output 1:
    NO

Input 2:
    A = [   [1, 2, 2, 1]
            [1, 1, 2, 2]
            [2, 1, 1, 2]    
            [1, 1, 2, 2]    ]
Output 2:
    YES
```
## Solution Approach

1. Create a string of first row elements and concatenate the string with itself.Let this string be temp.
2. Traverse all remaining rows. For every row being traversed, create a string s of current row elements. If s is not a substring of temp, return false.
3. Return true.

## Solution
### Editorial
```cpp
string Solution :: solve(vector <vector <int> > &A)
{
    int n = A.size();
    string temp = "";
    for (int i = 0 ; i < n ; i++)
    {
        temp = temp + "-" + to_string(A[0][i]);
    }
    temp += temp;
    for (int i=1; i<n; i++)
    {
        string s = "";
        for (int j = 0 ; j < n ; j++)
            {
             s = s + "-" +  to_string(A[i][j]);
            }
        if (temp.find(s) == string::npos)
        {
            return "NO";
        }
    }
    return "YES";
}
```

### Fastest
```cpp
string Solution::solve(vector<vector<int> > &A) 
{
    int i,j;
    string s="";
    for(i=0;i<A[0].size();i++)
        s=s+to_string(A[0][i])+"+";
    s=s+s;
    string s2="";
    for(i=1;i<A.size();i++)
    {
        s2="";
        for(j=0;j<A[0].size();j++)
            s2=s2+to_string(A[i][j])+"+";
        if(s.find(s2) > s.length())
            return "NO";
    }
    return "YES";
}
```

### Lightweight
```cpp
bool check(vector<int> v1,vector<int> v2,int size)
{
    
    for(int i=0;i<=size;i++)
    {
        int j=0;
        int k=i;
        int flag=0;
        while((k<2*size && j<size))
        {
            if(v1[k]==v2[j])
            {
                k++;j++;
                flag=1;
            }
            else
            {
                flag=0;
                break;
            }
        }
        if(flag==1)
        return true;
        
    }
    return false;
}
string Solution::solve(vector<vector<int> > &A) 
{
    string yes="YES";
    string no="NO";
    
    int rsize=A.size();
    int csize=A[0].size();
    
    vector<int> mains(0);
    for(int j=0;j<2;j++)
    {
    for(int i=0;i<csize;i++)
    {
        mains.push_back(A[0][i]);
    }
    }
    
    for(int i=1;i<rsize;i++)
    {
        if(check(mains,A[i],csize)==false)
        return no;
    }
    return yes;
}
```



