# Smallest Multiple With 0 and 1

https://www.interviewbit.com/problems/smallest-multiple-with-0-and-1/

You are given an integer N. You have to find smallest multiple of N which consists of digits 0 and 1 only. Since this multiple could be large, return it in form of a string.

Note:

Returned string should not contain leading zeroes.
For example,
```
For N = 55, 110 is smallest multiple consisting of digits 0 and 1.
For N = 2, 10 is the answer.
```

## Hint 1

Naive approach: We multiply N with positive numbers and then check it consists of only ones and zeros.

What could be a faster approach? A recursive solution where we build all possible numbers consists of digits ones and zeroes only.
For example, something like:
```
        1
       / \
      /   \
     /     \
    /       \
   10       11
  /  \     /  \
 /    \   /    \
100  101  110  111
```
And so on. How can we merge nodes in this based on values with modulo N in such a way that we don't visit all possible states.

## Solution Approach

Let's represent our numbers as strings here. Now, consider there are N states, where i'th state stores the smallest string which when take modulo with N gives i. Our aim is to reach state 0. Now, we start from state "1" and at each step we have two options, either to append "0" or "1" to current state. We try to explore both the options, but note that if I have already visited a state, why would I visit it again? It already stores the smallest string which achieves that state and if I visit it again with a new string it will surely have more characters than already stored string.

So, this is basically a BFS on the states. We'll visit a state atmost once, hence overall complexity is O(N). Interesting thing is that I need not store the whole string for each state, I can just store the value modulo N and I can easily see which two new states I am going to.

But, how do we build the solution?
If I reach a state x, I store two values

From which node I arrived at node x from. Say this is node y.
What(0 or 1) did I append at string at node y to reach node x
Using this information, I can build my solution by repeatedly going to parents starting from node 0.

## Solution

```cpp
string Solution::multiple(int N) {
    if(N==1) return "1";
    vector<int> p(N,-1);
    vector<int> s(N,-1);
    int steps[2] = {0,1};
    queue<int> q;
    q.push(1);

    while(!q.empty()){
        int curr = q.front();
        q.pop();
        if(curr==0) break;
        for(int step: steps){
            int next = (curr * 10 + step) % N;
            if(p[next]==-1){
                p[next]=curr;
                s[next]=step;
                q.push(next);
            }
        }
    }

    string number;
    for(int it=0; it!=1; it=p[it])
        number.push_back('0'+s[it]);
    number.push_back('1');
    return string(number.rbegin(), number.rend());
}
```


### Editorial

```cpp
typedef long long ll;
//flag stores which nodes i have already visited
//parent and val array as described in hints
vector<int> flag, parent, val;

string solve(int n) {
    int p, q, i, tot = 0;

    //final string
    string ret = "";

    //queue for bfs
    queue<int> myq;

    //initial node
    int temp = 1 % n;
    flag[temp] = 1;
    val[temp] = 1;
    myq.push(temp);

    while (1) {
        //pop from queue
        temp = myq.front();
        myq.pop();
        p = temp;

        //reached final state
        //build solution here
        if (p == 0) {
            p = 0;
            ret += (char)(val[p] + '0');
            p = parent[p];
            while (p != 0) {
                ret += (char)(val[p] + '0');
                p = parent[p];
            }
            reverse(ret.begin(), ret.end());
            return ret;
        }

        //visit two neighbors p*10 and p*10+1
        //if already not visited
        q = (p * 10) % n;
        if (flag[q] == 0)
            myq.push(q), flag[q] = 1, parent[q] = p, val[q] = 0;

        q++;
        if (q >= n) q -= n;
        if (flag[q] == 0)
            myq.push(q), flag[q] = 1, parent[q] = p, val[q] = 1;
    }
}

string Solution::multiple(int A) {
    flag.clear();
    parent.clear();
    val.clear();
    flag.resize(A);
    parent.resize(A);
    val.resize(A);
    return solve(A);
}

```
## Asked in
* Amazon
