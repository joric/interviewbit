# WiFi Routers

https://www.interviewbit.com/problems/wifi-routers

Given A users and B Wifi routers in an office. A matrix C of size A x 2 denoting the coordinates of 
users in 2D plane, where C[i][0] represents the x-coordinate and C[i][1] 
represents the y-coordinate of the i-th user.

A matrix D of size B x 2 denoting the coordinates of 
Wifi Routers in 2D plane, where D[i][0] represents the x-coordinate and C[i][1] 
represents the y-coordinate of the ith Wifi Router.

You have to make the office fully Wifi-enabled. Now each Wifi Router can only transmit data to a user or another WiFi Router only if the distance between the Router and the receiving user/router is not more than R. 
Each Wifi Router has the same range R. 
A Wifi Router can get data from the ground network or another Wifi Router. The aim here is that all the users get the data.

You have to find the minimal value of R that will 
ensure that all users get the data even if only one WiFi router is connected with the ground network.

Distance between two points P1 (x1,y1) and P2 (x2,y2) = sqrt ( (x1 - x2) * (x1 - x2) + (y1 - y2) * (y1 - y2) ).

The original answer can be a real value you have to return the square of the minimal value of R.

Note: No two routers/users occupy the same place.

### Input Format
```
The first argument given is an integer A.
The second argument given is an integer B.
The third argument given is matrix C.
The fourth argument given is matrix D.
```
### Output Format
```
Return the square of minimal R that ensures ensure that all users get the data even if only one WiFi router is 
connected with the ground network.
```

### Constraints
```
1 <= A, B <= 100
0 <= C[i][0], C[i][1], D[i][0], D[i][1] <= 100
```
### For Example
```
Input 1:
    A = 1
    B = 2
    C = [   [1, 1]   ]
    D = [   [0, 0]
            [0, 2]   ]
Output 1:
    2
    Explanation 1:
        Connect any of the two routers with the ground newtork.
        Minimal R = sqrt(2)
        so answer = sqrt(2) ^2 = 2.

Input 2:
    A : 7
    B : 3
    C : [   [5, 0]
            [9, 8]
            [9, 4]
            [6, 3]
            [1, 7]
            [7, 4]
            [4, 0]  ]
    D : [   [1, 0]
            [8, 8]
            [4, 7]  ]
Output 2:
    58
```

## Solution
### Editorial
```cpp
int func(int n,int m,vector<vector<int> > &C,vector<vector<int> > &D){
    int flag[109][109]={};
        vector<int> x;
        vector<int> y;
        vector<int> c;
        for(int i=0; i<n; i++){
            int temp1=C[i][0];
            int temp2=C[i][1];
            assert(temp1>=0 && temp1<=100);
            assert(temp2>=0 && temp2<=100);
            if(flag[temp1][temp2]==1)
                assert(-1>0);
            flag[temp1][temp2]=1;
            x.push_back(temp1);
            y.push_back(temp2);
            c.push_back(1);
        }
        for(int i=0; i<m; i++){
            int temp1=D[i][0];
            int temp2=D[i][1];
            assert(temp1>=0 && temp1<=100);
            assert(temp2>=0 && temp2<=100);
            if(flag[temp1][temp2]==1)
                assert(-1>0);
            flag[temp1][temp2]=1;
            x.push_back(temp1);
            y.push_back(temp2);
            c.push_back(0);
        }
        assert((int)c.size()==(n+m));
        int n1=(int)x.size();
        int d[n1][n1];
        for (int i=0; i<n1; i++)
            for (int j=0; j<n1; j++)
                d[i][j]=(x[i]-x[j])*(x[i]-x[j])+(y[i]-y[j])*(y[i]-y[j]);
        for (int i=0; i<n1; i++)  if (c[i]==0)
            for (int j=0; j<n1; j++)
                for (int k=0; k<n1; k++)
                    d[j][k]=min(d[j][k], max(d[j][i], d[i][k]));
        int sol=0;
        for (int i=0; i<n1; i++)
            for (int j=0; j<n1; j++)
                if (c[i]==0 && c[j]==1)
                    sol=max(sol, d[i][j]);
        assert(sol>=0&&sol<=INT_MAX);
        return sol;
}
int Solution::solve(int A, int B, vector<vector<int> > &C, vector<vector<int> > &D) {
    return func(A,B,C,D);
}
```

### Fastest
```cpp
int func(int n,int m,vector<vector<int> > &C,vector<vector<int> > &D){
    int flag[109][109]={};
        vector<int> x;
        vector<int> y;
        vector<int> c;
        for(int i=0; i<n; i++){
            int temp1=C[i][0];
            int temp2=C[i][1];
            assert(temp1>=0 && temp1<=100);
            assert(temp2>=0 && temp2<=100);
            if(flag[temp1][temp2]==1)
                assert(-1>0);
            flag[temp1][temp2]=1;
            x.push_back(temp1);
            y.push_back(temp2);
            c.push_back(1);
        }
        for(int i=0; i<m; i++){
            int temp1=D[i][0];
            int temp2=D[i][1];
            assert(temp1>=0 && temp1<=100);
            assert(temp2>=0 && temp2<=100);
            if(flag[temp1][temp2]==1)
                assert(-1>0);
            flag[temp1][temp2]=1;
            x.push_back(temp1);
            y.push_back(temp2);
            c.push_back(0);
        }
        assert((int)c.size()==(n+m));
        int n1=(int)x.size();
        int d[n1][n1];
        for (int i=0; i<n1; i++)
            for (int j=0; j<n1; j++)
                d[i][j]=(x[i]-x[j])*(x[i]-x[j])+(y[i]-y[j])*(y[i]-y[j]);
        for (int i=0; i<n1; i++)  if (c[i]==0)
            for (int j=0; j<n1; j++)
                for (int k=0; k<n1; k++)
                    d[j][k]=min(d[j][k], max(d[j][i], d[i][k]));
        int sol=0;
        for (int i=0; i<n1; i++)
            for (int j=0; j<n1; j++)
                if (c[i]==0 && c[j]==1)
                    sol=max(sol, d[i][j]);
        assert(sol>=0&&sol<=INT_MAX);
        return sol;
}
int Solution::solve(int A, int B, vector<vector<int> > &C, vector<vector<int> > &D) {
    return func(A,B,C,D);
}
```

### Lightweight
```cpp
int Solution::solve(int A, int B, vector<vector<int> > &C, vector<vector<int> > &D) {
    int L = 0, R = 20020;
    
    while(L<R) {
        int M = L + (R - L)/2;
        bool ok = true;
        for (int s=0;s<B&&ok;++s) {
            queue<int> qB;
            vector<char> usedB(B, 0);
            qB.push(s);
            usedB[s]=1;
    
            while(!qB.empty()) {
                int v = qB.front();
                qB.pop();
                
                for (int i=0;i<B;++i)if(!usedB[i]) {
                    int dx = D[i][0]-D[v][0], dy = D[i][1]-D[v][1]; 
                    if (dx*dx + dy*dy <= M)usedB[i]=1,qB.push(i);
                }
            }
            
            vector<char> usedA(A, 0);
            
            for (int j = 0; j < B; ++j) if (usedB[j]) {
                for (int i=0;i<A;++i) if (!usedA[i]) {
                    int dx = C[i][0]-D[j][0], dy = C[i][1]-D[j][1]; 
                    if (dx*dx + dy*dy <= M)usedA[i]=1;
                }
            }

            for (int i=0;i<A;++i) if (!usedA[i]) {
                ok = false;
                break;
            }
        }
        
        if (ok)R=M;
        else L=M+1;
    }
    
    return L;
}
```


