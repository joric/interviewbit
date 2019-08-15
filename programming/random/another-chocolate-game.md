# Another chocolate game

https://www.interviewbit.com/problems/another-chocolate-game/


Scooby is a very good mathematics teacher and that is why his students like him. It’s Scooby’s birthday and so he decided to gift chocolates to his students.

Scooby has a total of A different students and he buys B different chocolates for them. He wants to make sure that
each student gets exactly one chocolate. He is interested in knowing the number of ways in which these chocolates can be 
distributed to students. As the number of ways can be quite large, he is interested in knowing the number of ways modulo 
10^9+7.

### Constraints
```
Number of testcases T: 1<=T<=10000
Number of students of Scooby: 1<=N<=100000
Number of chocolates: 1<=M<=100000
```
### Input

```
An integer A
An integer B
```

### Note

1. Each student is different
2. Each chocolate is different
3. Each student should get exactly one chocolate
4. Your code will run against multiple test cases (The function in which you run your code will be called T times.)

### Output

One integer corresponding to the number of ways of distributing the choclates modulo 1000000007.

### Examples
```
Input:

3 
3
Output:

6
Explanation:

Let's say there are three students s1,s2 and s3 and there are three chocolates c1,c2 and c3.
Following is the possible distribution:
(s1=>c1,s2=>c2,s3=>c3)
(s1=>c1,s2=>c3,s3=>c2)
(s1=>c2,s2=>c1,s3=>c3)
(s1=>c2,s2=>c3,s3=>c1)
(s1=>c3,s2=>c1,s2=>c2)
(s1=>c3,s2=>c2,s1=>c1) 
```

## Hint 1

You basically should think in this way:-

You have A different students and B different chocolates.
So the first student has B options
The second student has B-1 options (Because the first student ate one chocolate and that chocolate is no more available.)
and so on.

## Solution Approach

You basically should think in this way:-

You have A different students and B different chocolates.
So the first student has B options
The second student has B-1 options (Because the first student ate one chocolate and that chocolate is no more available.)

We could easily figure out the answer which is as follows
B(B-1)(B-2)(B-3) ….(B-A+1)
We could easily compute the above multiplication under modulo.

## Solution

### Editorial

```cpp
/*
When B < A, the distribution is not possible and we return 0.
For B >= A, our Goal is to calculate V = B*(B-1)*(B-2)*..*(B-A+1)
we can write V = B!/(B-A)!

Observe that X! % M will always be non-zero for 0 <= X < M
Hence we conclude that (B-A)! has an inverse modulo M since 0 <= B-A < M
Using Fermat Little Theorem, we have:
power(X, M) = X % M
or, power(X, M-1) = 1 % M
or, power(X, M-2) = Y % M where Y is the inverse of X modulo M

Using this property, we can write

V = B! * power((B-A)!, M-2) % M
*/

const int M=1E9+7, N=1E5+5;
// Store factorial values in an array
long long F[N];
/* Time Complexity: O(N) */
void preProcess() {
    // To make sure run only once
    static int flag=0; if(flag++) return;
    // store factorial values in an array
    F[0]=1; for(long long i=1; i<N; i++) F[i]=F[i-1]*i%M;
}
/* Time Complexity: O(log A) */
long long I(long long A) {
    // this is when B < A
    if(A<0) return 0;
    // binary exponentiation to get power((B-A)!, M-2)
    long long X=F[A], Y=1, N=M-2;
    while(N) {
        if(N&1) Y=Y*X%M;
        if(N>>=1) X=X*X%M;
    }
    return Y;
}
/* Time Complexity: O(N + Q*log(max(A,B))) where
 * N = maximum possible value of A or B
 * Q = number of function calls made
 */
int Solution::solve(int A, int B) {
    preProcess();
    return F[B]*I(B-A)%M;
}
```

### Fastest
```cpp
#define mod 1000000007
vector<int> factorial(100001);
int flag = 0;
void Factorial(void)
{
    flag = 1;
    factorial[0] = 1;
    factorial[1] = 1;
    long long Fact = 1LL;
    for(int i = 2; i <= 100000; i++){
        Fact = (Fact*i)%mod;
        factorial[i] = Fact;
    }
}

long long Inverse(int n, int deg)
{
    long long res = 1LL, curr = n;
    while(deg){
        if(deg%2 != 0)
            res = (res*curr)%mod;
        deg/=2;
        curr = (curr*curr)%mod;
    }
    return res%mod;
    //if(deg == 0)
    //    return 1;
    //long long temp = Inverse(n,deg/2)%mod;
    
    // if(deg%2 == 0)
    //     return (temp*temp)%mod;
    // else
    //     return (n*((temp*temp)%mod))%mod;
}

int Solution::solve(int A, int B) {
    if(B < A)
        return 0;
        
    if(flag == 0)
        Factorial();
        
    if(A == B)
        return factorial[B];
        
    return (factorial[B]*Inverse(factorial[B-A],mod-2))%mod;
}
```
### Lightweight
```cpp
#define mod 1000000007
vector<int> factorial(100001);
int flag = 0;
void Factorial(void)
{
    flag = 1;
    factorial[0] = 1;
    factorial[1] = 1;
    long long Fact = 1LL;
    for(int i = 2; i <= 100000; i++){
        Fact = (Fact*i)%mod;
        factorial[i] = Fact;
    }
}

long long Inverse(int n, int deg)
{
    long long res = 1LL, curr = n;
    while(deg){
        if(deg%2 != 0)
            res = (res*curr)%mod;
        deg/=2;
        curr = (curr*curr)%mod;
    }
    return res%mod;
    //if(deg == 0)
    //    return 1;
    //long long temp = Inverse(n,deg/2)%mod;
    
    // if(deg%2 == 0)
    //     return (temp*temp)%mod;
    // else
    //     return (n*((temp*temp)%mod))%mod;
}

int Solution::solve(int A, int B) {
    if(B < A)
        return 0;
        
    if(flag == 0)
        Factorial();
        
    if(A == B)
        return factorial[B];
        
    return (factorial[B]*Inverse(factorial[B-A],mod-2))%mod;
}
```

### Mine
```cpp
public class Solution {
    public int solve(int A, int B) {
        int mod = 1000000007;
        if (A > B)
            return 0;
        else {
            long  result = 1;
            while (A!=0) {
                result = (result * B) % mod;
                A--;
                B--;
            }
            return (int)(result % mod);
        }
    }
}
```


