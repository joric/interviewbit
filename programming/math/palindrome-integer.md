# Palindrome Integer

https://www.interviewbit.com/problems/palindrome-integer


## Solution

```cpp
int Solution::isPalindrome(int A) {

    if(A<0)
        return false;

    long int temp = 0, temp2 = A;

    while(A) {
        temp = temp*10 + A%10;
        A /=10;
    }

    return temp==temp2;
}

/*
int Solution::isPalindrome(int A) {
    // 1 determine the bounds (number of digits)
    // 2 check all digits from edge to the middle
    
    if (A<0)
        return 0;
    
    int digits = 0, n=A;
    do {
        n/=10;
        digits++;
    } while(n);
    
    for (int i=0; i<digits/2; i++) {
        int leftpos = digits-i-1;
        int rightpos = i;
        
        int left = A/int(pow(10, leftpos))%10;
        int right = A/int(pow(10, rightpos))%10;
        
        //cout << "digits " << digits << " left " << left << " right " << right << endl;
        
        if (left != right)
            return 0;
    }
    return 1;
}
*/
```