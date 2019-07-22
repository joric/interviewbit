# Reverse Bits

https://www.interviewbit.com/problems/reverse-bits


## Solution

```cpp
unsigned int Solution::reverse(unsigned int A) {
    unsigned int res=0;
    int i, bit;
    for(i=0;i<32;i++) {
        bit = (A>>i) & 1;
        res = (res<<1) | bit;
    }
    return res;
}

unsigned int Solution::reverse(unsigned int A) {
    unsigned int res=0;
    for(int i=1; i<=32; i++) {
        res <<= 1;
        res |= A & 1;
        A >>= 1;
    }
    return res;
}


unsigned int Solution::reverse(unsigned int b) {
    unsigned int out = 0;
    int n = 32;

    if (b & 1)
        out |= 1;

    for (int i=1; i<n; i++) {
        b >>= 1;
        out <<= 1;
        if (b & 1)
            out |= 1;
    }
    
    return out;
}

unsigned int Solution::reverse(unsigned int x) {
    x = ((x & 0b11111111111111110000000000000000) >> 16)| ((x & 0b00000000000000001111111111111111) << 16);
    x = ((x & 0b11111111000000001111111100000000) >> 8) | ((x & 0b00000000111111110000000011111111) << 8);
    x = ((x & 0b11110000111100001111000011110000) >> 4) | ((x & 0b00001111000011110000111100001111) << 4);
    x = ((x & 0b11001100110011001100110011001100) >> 2) | ((x & 0b00110011001100110011001100110011) << 2);
    x = ((x & 0b10101010101010101010101010101010) >> 1) | ((x & 0b01010101010101010101010101010101) << 1);
    return x;
}

unsigned int Solution::reverse(unsigned int x) {
    // order doesn't matter (!)
    x = ((x & 0xaaaaaaaa) >> 1) | ((x & 0x55555555) << 1);
    x = ((x & 0xcccccccc) >> 2) | ((x & 0x33333333) << 2);
    x = ((x & 0xf0f0f0f0) >> 4) | ((x & 0x0f0f0f0f) << 4);
    x = ((x & 0xff00ff00) >> 8) | ((x & 0x00ff00ff) << 8);
    x = ((x & 0xffff0000) >> 16) | ((x & 0x0000ffff) << 16);
    return x;
}

unsigned int Solution::reverse(unsigned int b) {
    unsigned int out = 0;
    int n = 32;
    for (int i=0; i<n; i++) {
        if (b & (1<<i))
            out |= (1<<(n-i-1));
    }
    return out;
}
```