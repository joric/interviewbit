# Length of longest consecutive ones

https://www.interviewbit.com/problems/length-of-longest-consecutive-ones/

Given a binary string A. It is allowed to do at most one swap between any 0 and 1.
Find and return the length of the longest consecutive 1’s that can be achieved.

### Input Format

The only argument given is string A.

### Output Format

Return the length of the longest consecutive 1’s that can be achieved.

### Constraints

1 <= length of string <= 1000000

A contains only characters 0 and 1.

### For Example

```
Input 1:
    A = "111000"
Output 1:
    3

Input 2:
    A = "111011101"
Output 2:
    7
```

## Solution
### Editorial
```cpp
int Solution::solve(string s) {
    int n = s.length();
    int cnt_one = 0; 
  
    for (int i = 0; i < n; i++) { 
        if (s[i] == '1') 
            cnt_one++; 
    } 
   
    int left[n], right[n]; 
  
    if (s[0] == '1') 
        left[0] = 1; 
    else
        left[0] = 0; 
  
    if (s[n - 1] == '1') 
        right[n - 1] = 1; 
    else
        right[n - 1] = 0; 
  
    for (int i = 1; i < n; i++) { 
        if (s[i] == '1') 
            left[i] = left[i - 1] + 1;
        else
            left[i] = 0; 
    } 
  
    for (int i = n - 2; i >= 0; i--) { 
        if (s[i] == '1') 
            right[i] = right[i + 1] + 1; 
        else
            right[i] = 0; 
    } 
    int cnt = 0, max_cnt = 0 ;
    int flag =0; 
    for (int i = 1; i < n - 1; i++) { 
        if (s[i] == '0') { 
            int sum = left[i - 1] + right[i + 1]; 
            if (sum < cnt_one) 
                cnt = sum + 1; 
            else
                cnt = sum; 
            max_cnt = max(max_cnt, cnt); 
            cnt = 0; 
            flag = 1;
        } 
    } 
    if(flag==0)
        max_cnt = cnt_one;
  
    return max_cnt;  
}
```

### Fastest
```cpp

int Solution::solve(string A) {
    
    vector<int> vec;
    
    int count = 0;
    int count1 = 0;
    for(int i=0; i<A.size(); i++)
    {
        if(A[i]=='1')
            count++;
        else
        {
            vec.push_back(count);
            if(count!=0)
                count1++;
            count = 0;
        }
    }
    if(count!=0)
    {
        vec.push_back(count);
        count1++;
    }
    
    if(vec.empty())
        return 0;
        
    if(count1==1)
        return *max_element(vec.begin(), vec.end());
        
    if(count1>2)
    {
        int maxim = vec[0];
        for(int i=0; i<vec.size()-1; i++)
        {
            maxim = max(maxim, vec[i]+vec[i+1]+1);
        }
        
        return maxim;
    }
    
    int maxim = vec[0]+1;
    for(int i=0; i<vec.size()-1; i++)
    {
        if(vec[i]!=0 && vec[i+1]!=0)
            return vec[i]+vec[i+1];
            
        maxim = max(maxim, vec[i+1]+1);
    }
    return maxim;
}
```
### Lightweight
```cpp
int Solution::solve(string s) {
    int res = 1, n = s.size();
    // if (s[0]=='0') {
    //     int i = 1;
    //     while(i<n&&s[i]=='1')++i;
    //     res = i;
    // }
    
    // int a = 0;
    // while(a<n&&s[a]=='1')++a;
    
    int a = 0, cnt = 0, L = 0, R = 0;
    for(int i = 0; i < n; ++i)if(s[i]=='1')++R;
    for (int i = 0; i < n; ++i) {
        if (s[i]=='0')++cnt;
        else --R;
        while (a < i && cnt >= 2) {
            if (s[a]=='0')--cnt;
            else ++L;
            ++a;
        }
        int r = i - a + 1;
        if(cnt==0||max(L,R)>0)if (r > res) res = r;
    }
    
    return res;
}

```

## References
* https://www.geeksforgeeks.org/length-of-longest-consecutive-ones-by-at-most-one-swap-in-a-binary-string/

