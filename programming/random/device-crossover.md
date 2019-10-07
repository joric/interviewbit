# DEVICE CROSSOVER

https://www.interviewbit.com/problems/device-crossover/

CleverTap’s users can access the platform via a website and a mobile app. Every time the user
visits the website/mobile app, we store a timestamp of the visit.

Most of our users access the platform via the website only, however, there are a few who use
both mediums to access it. Given the visit history for a particular user, our product manager
would like to know how many times the user switched devices.

```
Example 1:
    9 am, 1 Apr - Website #1
    7 pm, 2 Apr - Website #2
    8 pm, 2 Apr - Website #3
    8 am, 7 Apr - App #4
    9 am, 7 Apr - Website #5
```
In the example above, the user switched devices twice - once from the Website to the App
(#3 to #4), and then from the App back to the Website (#4 to #5).

Example 2:
```
    1 am, 10 Apr - App #1
    7 pm, 18 Apr - Website #2
    8 pm, 28 Apr - Website #3
    8 am, 29 Apr - App #4
    9 am, 29 Apr - Website #5
```
In this example, the user switched devices thrice - once from the App to the Website (#1
to#2), and then from the Website to the App (#3 to #4), and finally from the App to the
Website (#4 to #5).

Given an array A of N integers, where each A[i] is the timestamp at which a user visited the website (sorted in ascending
order) and an another integar array B of size M, where each B[j] is the timestamp at which a user visited the app (sorted in ascending
order)

Return the number of switches between platforms.

### Input Format

* The first argument given is the integer array A.
* The second argument given is the integer array B.

### Output Format

Return the number of switches between platforms.

### Constraints

* 0 <= N, M <= 10^5
* 1 <= A[i],B[i] <= 10^7

### For Example
```
Input 1:
    A = [13, 14, 22]
    B = [8, 17]
Output 1:
    3
Explaination 1:
At time 8 user login at App
At time 13 user login at website #switch 1
At time 14 user login at website
At time 17 user login at App     #switch 2
At time 22 user login at website #switch 3

Input 2:
    A = [ ]
    B = [3, 5, 7, 9]
Output 2:
    0
```

## Solution
### Editorial
```cpp
int Solution::solve(vector <int> &A, vector <int> &B)
{
    int n = A.size();
    int m = B.size();
    int ptr1 = 0,ptr2 = 0;
    int ans = 0,last = 0;
    while(ptr1<n && ptr2<m)
    {
        if(A[ptr1] < B[ptr2])
        {
            if(last == 0 || last == 1)
            {
                last = 1;
            }
            else
            {
                last = 1;
                ans++;
            }
            ptr1++;
        }
        else
        {
            if(last == 0 || last == 2)
            {
                last = 2;
            }
            else
            {
                ans++;
                last = 2;
            }
            ptr2++;
        }
    }
    if(n!=0 && m!=0)
    {
        ans++;
    }
    return ans;
}
```

### Python
```python
class Solution:
    # @param A : list of integers
    # @param B : list of integers
    # @return an integer
    def solve(self, A, B):
        i = 0
        j = 0
        
        m = len(A)
        n = len(B)
        
        c = 0
        state = None
        while i < m and j < n:
            if A[i]< B[j]:
                if state is None:
                    state = "website"
                elif state =='app':
                    state = "website"
                    c+=1
                i+=1
            else:
                if state is None:
                    state ='app'
                elif state == 'website':
                    state='app'
                    c+=1
                j+=1
                
        if i == m:
            while j < n:
                if state == 'website':
                    state = 'app'
                    c+=1
                j+=1
                
        if j == n:
            while i<m:
                if state == 'app':
                    state = 'website'
                    c+=1
                i+=1
                
        return c
```

### Mine
```python
class Solution:
    def solve(self, A, B):
        x, res, last = {}, 0, -1
        for a in A: x[a] = 0
        for b in B: x[b] = 1
        for k,v in sorted(x.items()):
            if v != last and last!=-1:
                res += 1
            last = v
        return res
```

### Another Mine
```python
class Solution:
    def solve(self, A, B):
        i,j,m,n,c,state = 0,0,len(A),len(B),0,-1
        while i < m and j < n:
            oldstate = state
            if A[i]<B[j]:
                state = 0
                i+=1
            else:
                state = 1
                j+=1
            if oldstate!=state: c+=1
        if i<m and state==0: c+=1
        if j<n and state==1: c+=1
        return c
```
