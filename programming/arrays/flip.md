# Flip

https://www.interviewbit.com/problems/flip/

You are given a binary string(i.e. with characters 0 and 1) S consisting of characters S1, S2, ..., SN.
In a single operation, you can choose two indices L and R such that 1 <= L <= R <= N and flip the characters
S(L), S(L+1), ..., S(R). By flipping, we mean change character 0 to 1 and vice-versa.

Your aim is to perform ATMOST one operation such that in final string number of 1s is maximised.
If you don't want to perform the operation, return an empty array.
Else, return an array consisting of two elements denoting L and R.
If there are multiple solutions, return the lexicographically smallest pair of L and R.

Notes:

Pair (a, b) is lexicographically smaller than pair (c, d) if a < c or, if a == c and b < d.
For example,

```
S = 010

Pair of [L, R] | Final string
_______________|_____________
[1 1]          | 110
[1 2]          | 100
[1 3]          | 101
[2 2]          | 000
[2 3]          | 001
```

We see that two pairs [1, 1] and [1, 3] give same number of 1s in final string. So, we return [1, 1].
Another example,

If S = 111

No operation can give us more than three 1s in final string. So, we return empty array [].


## Hint 1
Note what is the net change in number of 1s in string S when we flip bits of string S. 
Say it has A 0s and B 1s. Eventually, there are B 0s and A 1s.

## Solution Approach

So, number of 1s increase by A - B. We want to choose a subarray which maximises this.
Note, if we change 1s to -1, then sum of values will give us A - B.
Then, we have to find a subarray with maximum sum, which can be done via Kadane's Algorithm.


## Solution

```cpp
/* editorial */

vector<int> Solution::flip(string A) {
    int n=A.length();

    //build new array of 1s and -1s
    vector<int> ar(n);
    for(int i=0; i<n; i++)
        if(A[i]=='1') ar[i]=-1;
        else ar[i]=1;

    //pair storing the answer
    pair<int, int> ans=make_pair(INT_MAX, INT_MAX);

    //basic kadane's algorithm implementation
    //we also storing the begin point for best till now
    int best_till_now=0,best_ending_here=0,l=0;
    for(int i=0; i<n; i++){
        if(best_ending_here+ar[i]<0){
            l=i+1;
            best_ending_here=0;
        }
        else best_ending_here+=ar[i];
        if(best_ending_here>best_till_now){
            best_till_now=best_ending_here;
            ans.first=l;
            ans.second=i;
        }
    }

    //if no valid subarray found
    if(ans.first==INT_MAX)return vector<int>();

    //return answer
    vector<int> ret;
    ret.push_back(ans.first+1);
    ret.push_back(ans.second+1);
    return ret;
}

/* something else */

vector <int> Solution::flip(string A) {
    int l = 0, lmax = -1, rmax = -1;
    int maxi = 0, cmax = 0;
    int len = A.length();
    
    for(int i=0; i<len; i++) {
        cmax += (A[i]=='0' ? 1 : -1);
        
        if(cmax > maxi) {
            maxi = cmax;
            lmax = l;
            rmax = i;
        }
        
        if(cmax < 0) {
            l = i + 1;
            cmax = 0;
        }
    }
    
    if(lmax == -1 || rmax == -1)
        return {};
    
    return {lmax+1, rmax+1};
}

/* find the subarray with MAX(no.of zeros-no. of ones) */

vector<int> Solution::flip(string A) {
    int ones = 0;
    int zeros = 0;
    int max = INT_MIN;
    int start = 0;
    int index = 0;
    int ind = 0;

    for(int i=0; i<A.size(); i++) {
        
        if(A[i]=='0'){
            zeros++;
        } else {
            ones++;
        }
        
        if (zeros-ones<0) {
            start = i + 1;
            zeros = 0;
            ones = 0;
        }
        
        if (max<(zeros-ones)) {
            max = zeros-ones;
            ind = start;
            index = i;
        }
    
    }
    
    if(max>0)
        return {ind+1, index+1};
    return {};
}


/* --- */

vector<int> Solution::flip(string A) {
    int n = A.length();
    int sum = 0, finalsum = 0;
    int start = 0, end = 0;
    int x = -1, y = -1;
    
    for (int i = 0; i < n; i++) {
        if (A[i] == '0')
            sum += 1;
        else 
            sum -= 1;
        
        if (sum < 0) {
            sum = 0;
            start = i + 1;
        }
        
        if (sum > finalsum) {
            finalsum = sum;
            x = start;
            y = end = i;
        }
    }
    
    if (x != -1 && y != -1) {
        x++;
        y++;
        return {x, y};
    } 
    
    return {};
}

/*---*/

vector<int> Solution::flip(string A) {
    int a, b, sum, max, tmp;
    sum = max = 0;
    a = b = tmp = 1;
    
    for(int i=0; i<A.size(); i++) {
        if(A[i]=='1') {
            if(!sum) {
                tmp = i + 2;
                continue;
            }
            sum--;
        } else {
            sum++;
            if(sum>max) {
                max=sum;
                b = i + 1;
                a = tmp;
            }
        }
    }
    if(!max) return {};
    return {a, b};
}
```

## Asked in

* VMWare
