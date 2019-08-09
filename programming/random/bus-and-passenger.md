# Bus and Passenger

https://www.interviewbit.com/problems/bus-and-passenger/

There is bus having N rows, each row consist of two seats of equal size. 
Given an array A where A[i] determines the width of seats in the ith row.

There are 2*N passengers of type 0 and type 1 (exactly N passengers of type 0 and exactly N passengers of type 1).

#### Type 0

Type 0 passenger always chooses a row where both seats are empty. 
Among these rows, he chooses the one with the smallest seat width and takes one of the seats in it.

#### Type 1

Type 1 always chooses a row where exactly one seat is occupied (by a type 0 passenger). 
Among these rows, he chooses the one with the largest seat width and takes the vacant place in it.

You are given a string B determining the order the passenger entering the bus where 
B[i] is either ‘0’ (type 0 passenger) or ‘1’ (type 1 passenger).

Return an array of intergers C, where C[i] determines row taken by ith passenger.

### Input Format

The argument given is the integer array A and string B.

### Output Format

Return an array of integers C, where C[i] determines row taken by passenger i.

### Constraints
```
1 <= length of the array <= 100000
1 <= A[i] <= 10^5
length of string = 2 * length of array
B[i] is either '0' or '1'.
All array elements are distinct.
```

### For Example

```
Input 1:
    A = [3, 1]
    B = "0011"
Output 1:
    C= [2, 1, 1, 2]

Input 2:
    A = [10, 8, 9, 11, 13, 5]
    B = "010010011101"
Output 2:
    C= [6, 6, 2, 3, 3, 1, 4, 4, 1, 2, 5, 5]
```
## Hint 1

Note that the final type 0 passenger-type 1 passenger pairs are uniquely determined, and that using the stack, it is possible to recover which type 1 passenger to which type 0 passenger will sit (note that the zeros and ones will form the correct bracket sequence). Then one of the solutions may be as follows:

1. Sort the array of the lengths of the rows in ascending order.
2. For each introvert write the number of the next free row and add it to the stack.
3. For each extrovert write the last number from the stack and remove it from there.

Time Complexity - nlogn


## Solution

### Editorial
```cpp
vector<int> Solution::solve(vector<int> &A, string B) {
    map<int,int>m;
    map<int,int>::iterator it;
    for(int i=0;i<A.size();i++) m[A[i]]=i;
    
    vector<int>c;
    stack<pair<int,int>>s; it=m.begin();
    for(int i=0;i<B.size();i++){
        if(B[i]=='0'){
            c.push_back(it->second+1);
            int hall=it->first; int halt=it->second;
            s.push({hall,halt});
            it++;
        }
        else{
            c.push_back(s.top().second+1);
            s.pop();
        }
    }
    return c;
}
```

### Fastest
```cpp
vector<int> Solution::solve(vector<int> &A, string B) {
int n=A.size();
vector<int> ans;
pair<int,int> a[n+1];
    for(int i=1;i<=n;i++)
    {
        a[i].first=A[i-1];
        a[i].second=i;
    }
    sort(a+1, a+n+1);
    string s=B;
    int left=1;
    stack<int> st;
    for(int i=0;i<2*n;i++)
    {
        if(s[i]=='0')
        {
            ans.push_back(a[left].second);
            st.push(a[left].second);
            left++;
        }
        else
        {
            ans.push_back(st.top());
            st.pop();
        }
    }
    return ans;
}
```

### Lightweight
```cpp
vector<int> Solution::solve(vector<int> &A, string B) {
    int n=A.size();
     priority_queue <pair<int,int>, vector<pair<int,int>>, greater<pair<int,int>> > mini;
     priority_queue <pair<int,int>> max;
     int i,j;
     pair<int,int> p;
     for(i=0;i<n;i++){
         mini.push({A[i],i});
     }
     vector<int> C(2*n);
     for(i=0;i<2*n;i++){
         if(B[i]=='0'){
             p=mini.top();
             max.push(p);
             C[i]=p.second +1;
             mini.pop();
         }
         else{
             p=max.top();
             max.pop();
             C[i]=p.second +1;
         }
         
     }
     return C;
}
```

### Mine
```cpp
vector<int> Solution::solve(vector<int> &seats, string pass) {
    vector<int> res;
    stack<pair<int, int>> s;
    vector<pair<int, int>> v;
    for (int i = 0; i < seats.size(); i++)
        v.push_back(make_pair(seats[i], i + 1));
    sort(v.begin(), v.end());
    int k = 0;
    for (char c:pass) {
        if (c == '0') {
            res.push_back(v[k].second);
            s.push(v[k++]);
        } else {
            res.push_back(s.top().second);
            s.pop();
        }
    }
    return res;
}
```
