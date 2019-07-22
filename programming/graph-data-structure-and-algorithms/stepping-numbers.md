# Stepping Numbers

https://www.interviewbit.com/problems/stepping-numbers/

Given N and M find all stepping numbers in range N to M

The stepping number:
```
A number is called as a stepping number if the adjacent digits have a difference of 1.
e.g 123 is stepping number, but 358 is not a stepping number
```
Example:
```
N = 10, M = 20
all stepping numbers are 10 , 12 
```
Return the numbers in sorted order.

## Hint 1

Assume that we have a graph where the starting node is 0 and we need to traverse it from the start node to all the reachable nodes.

With the restrictions given in the graph about the stepping numbers, what do you think should be the restrictions defining the next transitions in the graph.

## Solution Approach


Lets take a example:
```
N = 10 M = 20
start node = 0
From 0, we can move to 1 2 3 4 5 6 7 8 9 [ these are not in our range so we don't add it ]
now from 1, we can move to 12 and 10 
from 2, 23 and 21
from 3, 34 and 32
...
and so on
```

10 and 12 are in our range, so the answer = 2.
** Happy Coding **


## Solution

### Editorial

```cpp
vector<int> Ans;
void dfs(int len, int N, int M, int num = 0) {
    if (num >= N && num <= M) {
        Ans.push_back(num);
    }
    if (len == 0)
        return;

    if (num == 0) {
        for (int i = 1; i <= 9; ++i) {
            dfs(len - 1, N, M, i);
        }
        return;
    }

    int last_dig = num % 10;
    if (last_dig == 0) {

        dfs(len - 1, N, M, (num * 10) + (last_dig + 1));

    } else if (last_dig == 9) {

        dfs(len - 1, N, M, (num * 10) + (last_dig - 1));

    } else {
        dfs(len - 1, N, M, (num * 10) + (last_dig - 1));
        dfs(len - 1, N, M, (num * 10) + (last_dig + 1));
    }
}

vector<int> Solution::stepnum(int N, int M) {
    int len = 0;
    int temp = M;
    while (temp) {
        temp /= 10;
        len++;
    }

    Ans.clear();
    dfs(len, N, M);
    sort(Ans.begin(), Ans.end());
    return Ans;
}
```

### Mine
```cpp
// https://www.interviewbit.com/problems/stepping-numbers/

void step(int A, int B, vector<int>& sol, long long int num){
    if(num > B){
        return;
    }
    
    int last = num%10;
    
    if(num >= A){
        sol.push_back(num);
    }
    
    if(last == 0){
        step(A, B, sol, num*10 + 1);
    }
    else if(last == 9){
        step(A, B, sol, num*10 + 8);
    }
    else{
        step(A, B, sol, num*10 + last - 1);
        step(A, B, sol, num*10 + last + 1);
    }
}

vector<int> Solution::stepnum(int A, int B) {
    // Do not write main() function.
    // Do not read input, instead use the arguments to the function.
    // Do not print the output, instead return values as specified
    // Still have a doubt. Checkout www.interviewbit.com/pages/sample_codes/ for more details
    
    vector<int> sol;
    vector<int> a;
    vector<int> b;
    
    if(A > B){
        return sol;
    }
    
    if(A == 0){
        sol.push_back(0);
    }
    
    for(int i = 1; i < 10; i++){
        step(A, B, sol, (long long int)i);
    }
    
    if(sol.size() != 0){
        sort(sol.begin(), sol.end());
    }
    
    return sol;
}

```

## Asked in

* Epic systems
