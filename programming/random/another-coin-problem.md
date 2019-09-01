# Another Coin Problem

https://www.interviewbit.com/problems/another-coin-problem/

The monetary system in ShadowLand is really simple and systematic. The locals only use coins. 
The coins come in different values. The values used are:

1, 10, 25, 100, 1000, 2500, 10000, 100000, 250000, 1000000, ...
Formally, for each K >= 0 there are coins worth 10K, and coins worth 25*100K.

Given an integer A denoting the cost of a car, 
find and return the smallest number of coins necessary to pay exactly the cost of the car 
(assuming you have a sufficient supply of coins of each of the types you will need).


### Input Format

The only argument given is integer A.

### Output Format

Return the smallest number of coins necessary to pay exactly the cost of the car.

### Constraints

1 <= A <= 2*10^9

### For Example

Input 1:
    A = 47
Output 1:
    5   (1 + 1 + 10 + 10 + 25)

Input 2:
    A = 9
Output 2:
    9   (1 * 9)

## Solution

### Editorial
```cpp
int Solution::solve(int A) {
    int count = 0;
    while(A > 0) {
        int ten_pow_k = (log10(A));
        int hundred_pow_k = (log(A)/log(100));
        
        int ten_pow_k_num = pow(10, ten_pow_k);
        int twenty_five_k = 25 * pow(100, hundred_pow_k);
        if(twenty_five_k > A) {
            twenty_five_k = 25 * pow(100, (hundred_pow_k - 1));
        }
        
        if(max(ten_pow_k_num, twenty_five_k) == ten_pow_k_num) {
            count += A/ten_pow_k_num;
            A %= ten_pow_k_num;
        } else {
            // two possible approaches, compute both
            
            int Acpy1 = A;
            int test_count1 = 0;
            
            int test_count2 = 0;
            // 1: extract max possible no. of 25*100^k coins and compute
            test_count1 += Acpy1/twenty_five_k;
            Acpy1 %= twenty_five_k;
            
            test_count1 += Acpy1/ten_pow_k_num;
            Acpy1 %= ten_pow_k_num;
            
            test_count1 += Acpy1/(ten_pow_k_num/10);
            Acpy1 %= (ten_pow_k_num/10);
            
            // 2: extract one less and compute. 
            // (A will always equal Acpy eventually)
            test_count2 += (A/twenty_five_k) - 1;
            A = (A%twenty_five_k) + twenty_five_k;
            
            test_count2 += A/ten_pow_k_num;
            A %= ten_pow_k_num;
            
            test_count2 += A/(ten_pow_k_num/10);
            A %= (ten_pow_k_num/10);
            
            // report smaller of the two
            count += min(test_count1, test_count2);
        }
        // cout<<count<<"\n";
    }
    
    return count;
}
```

### Fastest
```cpp
int Solution::solve(int A) {
    int dp[100];
    dp[0]=0;
    for(int i = 1; i < 100; ++i) {
        dp[i]=1+dp[i-1];
        if(i>=10)dp[i]=min(dp[i],1+dp[i-10]);
        if(i>=25)dp[i]=min(dp[i],1+dp[i-25]);
    }
    
    int res = 0;
    while (A) {
        res += dp[A%100];
        A/=100;
    }
    
    return res;
}
```

### Lightweight
```cpp
int Solution::solve(int A) {
    /*int c=0;
    int t=log10(A);
    if(A>25)
    {
        int k=A/25;
        c=c+k;
        A=A-(k*25);
    }
    if(A>10)
    {
        int k=A/10;
        c=c+k;
        A=A-(k*10);
    }
    c=c+A;
    return c;*/
    long dp[100];
    for(int i = 0; i < 100; i++)    
        dp[i] = 1000;
    dp[0] = 0;
    for(int i = 1; i < 100; i++)    
        dp[i]=dp[i-1]+1;
    for(int i = 10; i < 100; i++)    
        dp[i] = min(dp[i-10] + 1, dp[i]);
    for(int i = 25; i < 100; i++)    
        dp[i] = min(dp[i-25] + 1, dp[i]);
    long ans = 0;
    while(A > 0) 
    {
        ans += dp[(int)(A % 100)];
        A /= 100;
    }
    return ans;
}
```
