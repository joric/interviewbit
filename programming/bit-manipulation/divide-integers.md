# Divide Integers

https://www.interviewbit.com/problems/divide-integers

Divide two integers without using multiplication, division and mod operator.

Return the floor of the result of the division.

Example:
```
5 / 2 = 2
```
Also, consider if there can be overflow cases. For overflow case, return INT_MAX.

## Hint 1

dividend = answer * divisor + c

You need to find the answer here without using any of the operators mentioned in the question. Think about the binary expansion of answer.

We can work with bits without using the standard operators. If you can find what bits are set in answer you will be done.

## Solution Approach

Think in terms of bits.

1. How do you do the division with bits?

2. How do you determine the most significant bit in the answer?

Iterate on the bit position 'i' from 31 to 1 and find the first bit for which divisor«i is less than dividend.

3. How do you use (1) to move forward in similar fashion?


## Solution
### Editorial
```cpp
int Solution::divide(int A, int B) {
    long long int dividend = (long long int)A;
    long long int divisor = (long long int)B;
    long long int sign;                                             // this will store the sign of final result
    if (dividend < 0 && divisor < 0 || dividend > 0 && divisor > 0) //if both the dividend and divisor is of same sign then answer will be positive else negative
    {
        sign = 1;
    } else {
        sign = -1;
    }
    dividend = abs(dividend); // take absolute values because we have already taken care of sign of final answer
    divisor = abs(divisor);
    if (dividend == 0) // zero divided by any number is 0
    {
        return 0;
    } else if (divisor == 1) // Dividing a number by 1 gives the number itself
    {
        long long int h = sign * dividend; // Find the answer by multiplying with the sign
        if (h > INT_MAX || h < INT_MIN)    // check for overflow
        {
            return INT_MAX;
        } else {
            return h;
        }
    }
    /* Since we do not have to use multiplication, division and mod operator what we are gonna do is 
    make use of logarithms. We know that log(a/b)=log(a)-log(b). We will use this property */

    /* So our answer = a/b ... taking log on the both sides we get log(ans)=log(a/b) ...using the above property
       we get log(ans)=log(a)-log(b)...now we have to remove the log from left hand side to get the answer.
       we will get ans=e^(log(a)-log(b))l*/

    long long int ans = sign * (exp(log(dividend) - log(divisor)));
    if (ans > INT_MAX || ans < INT_MIN) // again check for overflow
    {
        return INT_MAX;
    } else {
        return ans; //return answer
    }
}

```

### Fastest
```cpp
int Solution::divide(int a, int b) {
   long long n = a, m = b;
   int sign = n < 0 ^ m < 0 ? -1 :1;
   long long q = 0;
   n = abs(n);
   m = abs(m);
   for (long long t = 0, i = 31; i >= 0; i--)
        if (t + (m << i) <= n)
            t += m << i, q |= 1LL << i;

    // assign back the sign
    if (sign < 0) q = -q;

    // check for overflow and return
    return q >= INT_MAX || q < INT_MIN ? INT_MAX: q;
}
```

### Lightweight
```cpp
int Solution::divide(int dividend, int divisor) {
    int sign = 1;
    if (dividend<0){sign = -sign;}
    if (divisor<0){sign = -sign;}
     
    unsigned long long tmp = abs((long long)dividend);
    unsigned long long tmp2 = abs((long long)divisor);
            
    unsigned long c = 1;
    while (tmp2 < tmp){
        tmp2 = tmp2<<1;    
        c = c<<1;
    }
     
    long long res = 0;
    while (tmp>=abs((long long)divisor)){
        while (tmp2 <= tmp){
            tmp -= tmp2;
            res = res+c;
        }
        tmp2=tmp2>>1;
        c=c>>1;    
    }
     
    return (sign*res >= INT_MAX ||  sign*res < INT_MIN) ? INT_MAX: sign*res;
}
```


## Asked in
* Microsoft
* Amazon
