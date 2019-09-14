# Number of Hosts per Subnet

https://www.interviewbit.com/problems/number-of-hosts-per-subnet/


Given a string A representing valid subnet mask. Find the number of hosts possible.


### Input Format

The only argument given is string A.

### Output Format

Return the number of possible hosts.

### For Example

```
Input 1:
    A = "255.255.248.0"
Output 1:
    2046

Input 2:
    A = "255.255.128.0"
Output 2:
    32766
```

## Solution
### Editorial
```cpp
int checkunset(int x){
    int notset=0;
    for(int i=0; i<8; ++i)
        if(x&(1<<i));
        else
            ++notset;
    return notset;
}

int solveit(string s){
    int n=s.size();
    int unset=0;
    int prev=0;
    for(int i=0; i<s.size(); ++i){
        if(s[i]=='.'){
            unset+=checkunset(prev);
            prev=0;
        }
        else
            prev=prev*10+(s[i]-'0');
    }
    unset+=checkunset(prev);
    if(unset<=1)
        return 0;
    int ans=1;
    for(int i=0; i<unset; ++i)
        ans=ans*2;
    return (ans-2);
}

int Solution::solve(string A) {
    return solveit(A);
}
```

### Mine
```cpp
int Solution::solve(string A) {
    unsigned int mask = 0;
    for (int i=0, pos=0; i<4; i++) {
        int p = A.find('.', pos);
        int d = atoi(A.substr(pos, p-pos).c_str());
        pos = p+1;
        mask = (mask << 8) | d;
    }
    
    int hosts = 1;
    while (!(mask & 1)) {
        hosts *= 2;
        mask >>= 1;
    }
    
    return hosts - 2;
}
```
### Another Mine
```cpp
int add_bits(int hosts, unsigned char x) {
    for (int i=0; i<8; i++) {
        if ((x & 1)==0) hosts *=2; else break;
        x >>= 1;
    }
    return hosts;
}

int Solution::solve(string A) {
    int hosts = 1, d = 0;
    for (char c:A) if (c=='.') hosts = add_bits(hosts, d), d = 0; else d = d*10 + c-'0';
    return add_bits(hosts, d) - 2;
}
```
