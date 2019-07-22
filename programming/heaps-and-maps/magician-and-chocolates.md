# Magician and Chocolates

https://www.interviewbit.com/problems/magician-and-chocolates



Given N bags, each bag contains Ai chocolates. There is a kid and a magician.
In one unit of time, kid chooses a random bag i, eats Ai chocolates,
then the magician fills the ith bag with floor(Ai/2) chocolates.

Given Ai for 1 <= i <= N, find the maximum number of chocolates kid can eat in K units of time.



It is quite trivial to figure out that the kid will always choose the bag with the maximum number of chocolates.
By knowing this fact, how would you solve the problem ?

The solution to this problem can be found greedily. At any time t, the kid will always choose the bag with the maximum number of chocolates and consume all it's chocolates. 
So we need to maintain the current maximum size among all bags for every time t = 1, ... , K and also updating the sizes of the bags.
This can be done using a max heap: https://en.wikipedia.org/wiki/Min-max_heap

## Solution

```cpp

// editorial

long long int mod = 1000000007;
int Solution::nchoc(int A, vector<int> &B) {
    int N = B.size();
    int K = A;
    long long int ans = 0;
    priority_queue<int> heap(B.begin(),B.end());
    while(K--){
        long long int max_elem = heap.top();
        ans += max_elem;
        ans = ans % mod;
        heap.pop();
        heap.push((int)(max_elem/2));
    }   
    return ans;
}


```