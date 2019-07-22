# Sum Of Fibonacci Numbers


How many minimum numbers from fibonacci series are required such that sum of numbers should be equal to a given Number N?
Note : repetition of number is allowed.

Example:

```
N = 4
Fibonacci numbers : 1 1 2 3 5 .... so on
here 2 + 2 = 4 
so minimum numbers will be 2 
```

## Hint 1

### Approach 1

Can you apply shortest path algorithm for finding solution

start vertex = 0
end vertex = N
Ans = shortest path from start vertex to end_vertex

Can you define moves ??

### Approach 2

Dynamic programming. 
Lets say you knew the minimum numbers required for all numbers < N. Can you use that to figure out the answer for N ?

### Approach 3

If you do look at either of Approach 1 or Approach 2, try to see for a pattern. 
Could greedy approach work here ?

## Solution Approach


### Approach 1

lets take an exmaple:

```
N = 10
Fibonacci elements : 1 1 2 3 5 8 ... so on
Assume:
start vertex = 0
end vertex = 10

Lets do a Breadth First Search. 

we can go from 0 to 1 2 3 5 8 .... so on
So, there will be an edge of weight 1 from 0 to vertices 1, 2, 3, 5, 8 ( We don't care about vertices > 10 for this case ).

Then from 1 2 3 5 8 
we can go to 2 4 5 10  ( We do not go to 3 again, because its already visited ). 

Thus, we reach at 10 in only 2 step so our answer will be 2
```

How would this approach work for big numbers ?

### Approach 2

ANSWER[N] = MIN ( ANSWER(N - FIB[I]) for I = 1 to X where FIB[X + 1] > N and FIB[X] <= N )

Does this approach work though if the numbers are really big ?

### Approach 3

Greedy is the winner here. 
Because of how the fibonacci numbers behave, the approach of picking the largest number less than or equal to the number N works.

So, an approach like :

```
solve(N) : 
  find F as the largest Fib <= N. 
  return solve(N-F) + 1
```

Can you prove why greedy approach will work here ?


## Solution

### Editorial
```cpp
int Solution::fibsum(int N) {
    vector<int> Fib;
    Fib.push_back(1);
    Fib.push_back(1);
    for(int i = 2; Fib[i - 1] < N; ++i) {
        int data = Fib[i - 1] + Fib[i - 2];
        Fib.push_back(data);
    }
    
    int ans = 0;
    int index = Fib.size() - 1;
    while (N > 0) {
        // find the largest fibonacci less than N.
        while (Fib[index] > N) index--;
        N -= Fib[index];
        ans++;
    }
    return ans;
    }
```

### Mine

```cpp
int Solution::fibsum(int n) {
    vector <int> fib;
    fib.push_back(1);
    fib.push_back(1);

    for(int i=2;fib[i-1]<n;i++) {
        int val=fib[i-1]+fib[i-2];
        fib.push_back(val);
    }

    int ans = 0;
    int size = fib.size();
    size--;

    while(n>0) {
        while(fib[size]>n)
            size--;
        n = n-fib[size];
        ans++;
    }
    return ans;
}

```