# Falling Squares

https://www.interviewbit.com/problems/falling-squares/?ref=random-problem


On an infinite number line (x-axis), we drop given squares in the order they are given.

The i-th square dropped (A[i] = (left, side_length)) is a square with the left-most point being A[i][0] and 
side length A[i][1].

The square is dropped with the bottom edge parallel to the number line, and from a higher height than all currently 
landed squares. We wait for each square to stick before dropping the next.

The squares are infinitely sticky on their bottom edge, and will remain fixed to any positive length surface they touch 
(either the number line or another square). Squares dropped adjacent to each other will not stick together prematurely.

Given A, find and return a list of heights H. Each height H[i] represents the current highest height of any 
square we have dropped, after dropping squares represented by A[0], A[1], …, A[i].



### Input Format

The only argument given is the integer matrix A.


### Output Format

Return the array H.

### Constraints
```
1 <= length of the array A <= 1000
1 <= A[i][0] <= 10^8
1 <= A[i][1] <= 10^6
```
### For Example
```
Input 1:
    A = [[1, 2], [2, 3], [6, 1]]
Output 1:
    [2, 5, 5]

Input 2:
    A = [[100, 100], [200, 100]]
Output 2:
    [100, 100]

```

## Solution
### Editorial
```cpp
vector<int> Solution::solve(vector<vector<int> > &A) {
    int n = A.size();
    vector<int> qans(n);
    for(int i = 0; i < n; i++){
        int left = A[i][0];
        int size = A[i][1];
        int right = left + size;
        qans[i] += size;
        for(int j = i + 1; j < n; j++){
            int left2 = A[j][0];
            int size2 = A[j][1];
            int right2 = left2 + size2;
            if(left2 < right && left < right2){
                qans[j] = max(qans[j], qans[i]);
            }
        }
    }
    vector<int> ans;
    int cur = -1;
    for(int x: qans){
        cur = max(cur, x);
        ans.push_back(cur);
    }
    return ans;
}
```

### Fastest
```cpp
vector<int> fallingSquares(vector<pair<int, int>>& positions) {
    std::map<int, int> distro = { { 0, 0 }, { INT_MAX, 0 } };
    int n = positions.size();
    vector<int> result(n, 0);
    
    int maxH = 0, k = 0;
    for (auto& p : positions) {
        int start = p.first, end = p.first + p.second;
        auto itr1 = distro.upper_bound(start), itr2 = distro.lower_bound(end);
        
        int borderH = itr2->first == end ? itr2->second : (--itr2)++->second;
    
        int localMaxH = 0;
        for (auto iter = --itr1; iter != itr2; ++iter) {
            localMaxH = std::max(localMaxH, iter->second);
        }
        localMaxH += p.second;
        
        distro.erase(++itr1, itr2);
        
        distro[start] = localMaxH;
        distro[end] = borderH;
        
        maxH = std::max(maxH, localMaxH);
        result[k++] = maxH;
    }
    return result;
}

vector<int> Solution::solve(vector<vector<int> > &A) 
{
    
    vector<pair<int, int>> p;
    
    for(int i=0; i<A.size(); i++)
    {
        
        p.push_back(make_pair(A[i][0], A[i][1]));
        
    }
    
    return fallingSquares(p);
    
}
```
### Lightweight
```cpp
vector<int> Solution::solve(vector<vector<int> > &A) {
    int n=A.size();
    vector<int>arr(n);
    vector<int>res(n);
    arr[0]=A[0][1];
    res[0]=A[0][1];
    int ma=A[0][1];
    for(int i=0;i<n;i++)
    {
        A[i][1]=A[i][0]+A[i][1];
    }
    for(int i=1;i<n;i++)
    {
        int my=A[i][1]-A[i][0];
        for(int j=0;j<i;j++)
        {
            if(A[i][0]>=A[j][0] && A[i][0]<A[j][1] || A[i][1]<=A[j][1] && A[i][1]>A[j][0]  || A[i][0]<=A[j][0] && A[i][1]>=A[j][1])
            {
                int h=A[i][1]-A[i][0];
                my=max(my,arr[j]+h);
            }
        }
        arr[i]=my;
        if(ma<my)
        {
            ma=my;
        }
        res[i]=ma;
    }
    // for(int i=0;i<n;i++)
    // {
    //     cout<<res[i]<<" ";
    // }
    return res;
}

```
