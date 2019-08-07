# Scores

https://www.interviewbit.com/problems/scores/

You are playing a game called "Ball Throw". Each throw has value of either 2 or 3 points.

1. A throw is worth 2 points if the throw distance is less than or equal to "D" meters.

2. A throw is worth 3 points if the throw distance is larger than "D" meters, where "D" is some non-negative integer.
There are 2 teams competing and you need to get the maximum advantage for your team i.e (the points of the first team minus the points of the second team) has to be maximum.

Choose some value of D that would do the task and tell the the scores for both team for that particular value of D.

```
Input

The first argument is an array of length N (1 <= N <= 200000) - the number of throws of the first team.
It contains array of N integer numbers — the distances of throws Ai (1 <= Ai <= 2000000000).

The second argument is an array of length M (1 <= M <= 200000) - the number of throws of the second team.
It contains array of M integer numbers — the distances of throws Bi (1 <= Bi <= 2000000000).

Output

Return array of two elements "A" "B" where "A" is first team's score and "B" is second teams score
with an optimal "D".  If there are several values of "D" which optimize "A-B",
find the one in which "A" is maximum.

Examples

Input

1 2 3
5 6
Output

9 6
Example Explanation: We can keep D = 0. Team A will then score 3 points on each of their
throws with final score as 9. Team B scores 6. The difference is then 3 which
is the most optimal difference possible.

Input

6 7 8 9 10
1 2 3 4 5
Output

15 10
```

## Hint 1
	
Think a greedy approach and try sorting the arrays. See if we can find the particular value of a that gives maximum advantage for the team.

## Solution Approach

Combine both the arrays and mark each throw as for first or second team and sort them. Now take the initial answer for both teams as we get for full 3 points for each throw. Now start traversing the sorted array from backward. Check if the current team is first or second and update the answer accordingly.



## Solution

### Editorial
```cpp
// int lesser(int d,vector<int> &A)
// {
//     int n=A.size();
//     int i=0;
//     int c_less_a=0;
//     while(A[i]<=d && i<n)
//     {
//         c_less_a++;
//         i++;
//     }
//     return c_less_a;
// }
vector<int> Solution::solve(vector<int> &A, vector<int> &B) {
    sort(A.begin(),A.end());
    sort(B.begin(),B.end());
    
    int n1=A.size();
    int n2=B.size();
    
    int start=min(A[0],B[0])-1;
    int end=max(A[n1-1],B[n2-1])+1;

    int max_res=INT_MIN;
    int max_a=0;
    int max_b=0;
    
    int ind_a=0;
    int prev_a=0;
    int ind_b=0;
    int prev_b=0;
    
    for(int d=start;d<=end;d++)
    {
        int i=ind_a;
        int less_a=prev_a;
        while(A[i]<=d && i<n1)
        {
            less_a++;
            i++;
        }
        prev_a=less_a;
        ind_a=i;
        
        int j=ind_b;
        int less_b=prev_b;
        while(B[j]<=d && j<n2)
        {
            less_b++;
            j++;
        }
        prev_b=less_b;
        ind_b=j;
        
        // cout<<"for d: "<<d<<" /less than d in a :"<<less_a<<" /less than d in b "<<less_b<<endl;
        int greater_a=n1-less_a;
        int greater_b=n2-less_b;
        
        int score_a=less_a*2+greater_a*3;
        int score_b=less_b*2+greater_b*3;
        
        if((score_a-score_b)>=max_res)
        {
            if((score_a-score_b)==max_res)
            {
                if(score_a>max_a)
                {
                    max_res=score_a-score_b;
                    max_a=score_a;
                    max_b=score_b;
                }
            }
            else
            {
                max_res=score_a-score_b;
                max_a=score_a;
                max_b=score_b;
            }// the_d=d;
        }
    }
    
    // cout<<the_d<<"\n";
    vector<int> ans;
    ans.push_back(max_a);
    ans.push_back(max_b);
    return ans;
}

```
### Fastest
```cpp
vector<int> Solution::solve(vector<int> &A, vector<int> &B) {
    pair<int,int>a[10004];
    int n=A.size();
    int m=B.size();
    for(int i=0;i<n;i++)
    {
        a[i].first=A[i];
        a[i].second=-1;
    }
    for(int i=0;i<m;i++){
        a[i+n].first=B[i];
        a[i+n].second=1;
        
    }
    sort(a,a+n+m);
    int x=n*3,mx,my;
    int y=m*3;
    mx=x;
    my=y;
    for(int i=0;i<m+n;i++){
        if(a[i].second==1)
        y--;
        else
        x--;
        if(x-y>mx-my)
        {
            mx=x;
            my=y;
        }
    }
    vector<int>ans;
    ans.push_back(mx);
    ans.push_back(my);
    return ans;
}

```

### Lightweight

```cpp
int binary(int s,vector<int> &B){
    int start=0;int last=B.size();
    while(start<last){
        int mid=(start+last)/2;
        if(B[mid]<=s)start=mid+1;
        else last=mid;
    }
    return start;
}
vector<int> Solution::solve(vector<int> &A, vector<int> &B) {
    int n=A.size(),m=B.size();
    int ans=INT_MIN;
    int start=-1;
    int last =-1;
    sort(A.begin(),A.end());
    sort(B.begin(),B.end());
    
    for(int i=0;i<n;i++){
        int t=binary(A[i],B);
        while(A[i]==A[i+1]&&i+1<A.size())i++;
        if(ans< (2*(i+1-t)+3*(n-i-1-m+t))){
            start=2*(i+1)+3*(n-i-1);
            last=2*(t)+3*(m-t);
            ans=start-last;
            //cout<<start<<"  "<<last<<" ";
        }
        else if(ans==(2*(i+1-t)+3*(n-i-1-m+t) &&start<2*(i+1)+3*(n-i-1))){
            start=2*(i+1)+3*(n-i-1); last=2*(t)+3*(m-t);}
    }
    //cout<<ans<<endl;
    int t=binary(*min_element(A.begin(),A.end())-1,B);
    //cout<<t<<endl;
    if(ans<(2*(-t)+3*(n-m+t))){ans=3*(n-m);return {3*(n),2*(t)+3*(m-t)};}
    else if(ans==(2*(-t)+3*(n-m+t)) &&start<3*(n)){
            start=3*(n); last=2*(t)+3*(m-t);}
    
    
    for(int i=0;i<m;i++){
        int t=binary(B[i],A);
        while(B[i]==B[i+1]&&i+1<B.size())i++;
        if(ans< (2*(-i-1+t)+3*(-m+i+1+n-t))){
            last=2*(i+1)+3*(m-i-1);
            start=2*(t)+3*(n-t);
            ans=start-last;
            //cout<<start<<"  "<<last<<" ";
        }
        else if(ans==(2*(-i-1+t)+3*(-m+i+1+n-t)) &&start<2*(t)+3*(n-t)){
            last=2*(i+1)+3*(m-i-1); start=2*(t)+3*(n-t);}
    }
    
     t=binary(*max_element(B.begin(),B.end())+1,A);
   // cout<<t<<endl;
    if(ans<(2*(t-m)+3*(n-t))){ans=2*(t-m)+3*(n-t);return {2*(t)+3*(n-t),2*m};}
    else if(ans==(2*(t-m)+3*(n-t)) &&start<2*(t)+3*(n-t)){
            start=2*(t)+3*(n-t); last=2*m;}
            
    return {start,last};
}

```

### Another one

```cpp
vector<int> Solution::solve(vector<int> &A, vector<int> &B) {
    vector<pair<int,int>> v;
    for(int i=0;i<A.size();i++){
        v.push_back(make_pair(A[i],0));
    }
    for(int i=0;i<B.size();i++){
        v.push_back(make_pair(B[i],1));
    }
    sort(v.begin(),v.end());
    long long maxA=3*A.size(),maxB=3*B.size();
    if(maxA-maxB<0){
        maxA=2*A.size(),maxB=2*B.size();
    }
    vector<int> ans;
    int x=0,y=0;
    long long sumA,sumB;
    int i=v.size()-1;
    while(i){
        if(v[i].second==0){
            x++;
        }
        else{
            y++;
        }
        sumA=3*x+2*(A.size()-x);
        sumB=3*y+2*(B.size()-y);
        if(sumA-sumB>maxA-maxB){
            maxA=sumA;
            maxB=sumB;
        }
        else if(sumA-sumB==maxA-maxB && sumA>maxA){
            maxA=sumA;
            maxB=sumB;
        }
        i--;
    }
    
    ans.push_back(maxA);
    ans.push_back(maxB);
    return ans;
}
```
