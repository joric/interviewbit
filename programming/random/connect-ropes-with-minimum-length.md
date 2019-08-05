# Connect ropes with minimum length

https://www.interviewbit.com/problems/connect-ropes-with-minimum-length


Given an array of integers A representing the length of ropes.
You need to connect these ropes into one rope. 
The cost of connecting two ropes is equal to the sum of their lengths.
You need to connect the ropes with minimum cost.

Find and return the minimum cost to connect these ropes into one rope.

### Input Format

The only argument given is the integer array A.

### Output Format

Return the minimum cost to connect these ropes into one rope.

### Constraints
```
1 <= length of the array <= 100000
1 <= A[i] <= 10^3
```
### For Example
```
Input 1:
    A = [1, 2, 3, 4, 5]
Output 1:
    33

Input 2:
    A = [5, 17, 100, 11]
Output 2:
    182
```
## Solution
### Editorial
```cpp
int Solution::solve(vector<int> &A) {
    priority_queue<int,vector<int>,greater<int>> pq;
    for(int i=0;i<A.size();i++){
        pq.push(A[i]);
    }
    int ans = 0;
    while(pq.size()>1){
        int val1= pq.top();
        pq.pop();
        int val2= pq.top();
        pq.pop();
        ans += val1+val2;
        pq.push(val1+val2);
    }
    return ans ;
}
```
### Fastest
```cpp
int Solution::solve(vector<int> &A) {
    map<int,int> m;
    int cost = 0;
    for(int i = 0;i < A.size();++i) ++m[A[i]];
    map<int,int>::iterator itr = m.begin();
    while(m.size() > 1){
        itr = m.begin();
        int num1 = itr->first;
        if(itr->second <= 1) m.erase(num1);
        else itr->second = itr->second - 1;
        if(itr == m.end()) break;
        if(itr->second <= 1) itr = m.begin();
        int num2 = itr->first;
        if(itr->second <= 1) m.erase(num2);
        else itr->second = itr->second - 1;
        cost += (num1 + num2);
        //cout << cost << endl;
        ++m[num1 + num2];
    }
    return cost;
}
```
### Lightweight
```cpp
void heapify(vector<int>& v,int i,int size)
{
    int left=2*i+1;
    int right=2*i+2;
    int pos=i;
    if(left<size && v[left]<v[pos])
    {
        
        pos=left;
    }
    if(right<size && v[right]<v[pos])
    {
        
        pos=right;
    }
    
    if(i!=pos)
    {
        int temp=v[i];
        v[i]=v[pos];
        v[pos]=temp;
        heapify(v,pos,size);
    }
}
int Solution::solve(vector<int> &A) 
{
    int sum=0;
    int size=A.size();
    if(size==1)
    return A[0];
    if(size==2)
    return A[0]+A[1];
    for(int i=size/2-1;i>=0;i--)
    {
        heapify(A,i,size);
    }
    
    for(int i=0;i<size-1;i++)
    {
        int sum1=0;
        heapify(A,0,size);
        //cout<<A[0]<<" ";
        //print(A,size);
        sum1=sum1+A[0];
        A[0]=INT_MAX;
        heapify(A,0,size);
        sum1=sum1+A[0];
        //print(A,size);
        //cout<<A[0]<<" ";
        //cout<<sum1<<endl;
        A[0]=sum1;
        sum=sum+sum1;
    }
    
    return sum;
    
}
```

### Mine
```cpp
int Solution::solve(vector<int> &arr) {
    priority_queue<int, vector<int>, greater<int> > pq(arr.begin(), arr.end()); 
    int res = 0; 
    while (pq.size() > 1) { 
        int first = pq.top(); 
        pq.pop(); 
        int second = pq.top(); 
        pq.pop(); 
        res += first + second; 
        pq.push(first + second); 
    } 
    return res;
}
```

