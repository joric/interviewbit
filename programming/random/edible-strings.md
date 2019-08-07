# EDIBLE STRINGS

https://www.interviewbit.com/problems/edible-strings/

Given a string of lowercase alphabets A of size N.

A string is edible if it has vowels('a', 'e', 'i', 'o', 'u') at only prime indexes.

Return a string B, Where B = "YES" if A is edible else B = "NO".

### Input Format

The first and only argument given is the string A.

### Output Format

Return a string B,Where B = "YES" if A is edible else B = "NO".

### Constraints

```
1 <= N <= 10^5
```

### For Example
```
Input 1:
    A = "saurabh"
Output 1:
    YES
Explaination 1:
    A contains vowels at indexes 2, 3 and 5 and all of them are prime, hence making string A edible.

Input 2:
    A = "sorabh"
Output 2:
    NO
Explaination 2:
    A contains vowels at indexes 2 and 4 and all of them are not prime, hence string A is not edible.
```

## Solution

### Editorial
```cpp

bool vowel(char c){
    return (c=='a' || c=='e' || c=='i' || c=='o' || c=='u');
}

string Solution::solve(string A){
    int n = A.size();
    vector<bool> pe(n+1, true);
    pe[0] = false, pe[1] = false;
    for(int i=2;i<=sqrt(n);i++){
        if(pe[i]){
            for(int j=i*i;j<=n;j+=i){
                pe[j]=false;
            }
        }
    }
    
    for(int i=0;i<n;i++){
        if(vowel(A[i]) && !pe[i+1]) return "NO";
    }
    return "YES";
}
```

### Fastest
```cpp
const int N=100005;

int p[N];
int tc=0;
void pre(){
    if(tc)
        return;
    p[1]=1;
    tc=1;
    for(int i=4; i<N; i+=2)
        p[i]=1;
    for(int i=3; i*i<N; i+=2)
        if(!p[i])
            for(int j=i*i; j<N; j+=i+i)
                p[j]=0;
}

bool vowel(char a){
    return (a=='a'||a=='e'||a=='i'||a=='o'||a=='u');
}

string Solution::solve(string A){
    pre();
    for(auto &it:A)
        assert(it>='a'&&it<='z');
    assert(A.size()>=1&&A.size()<=100000);
    int y=1;
    for(int i=1; i<A.size()+1; ++i){
        if(vowel(A[i-1])&&p[i]==1){
            y=0;
            break;
        }
    }
    if(y)
        return "YES";
    else
        return "NO";
    
}
```

### Lightweight
```cpp
int isPrime(int A) {
    if (A == 1) return 0;
    if (A == 2) return 1;
    for (int i = 2; i <= (int)sqrt(A)+1; i++) {
        if (A % i == 0)
            return 0;
    }
    return 1;
}
string Solution::solve(string A) {
    for (int i = 0; i < A.length(); ++i) {
        if (A[i] == 'a' ||
            A[i] == 'e' ||
            A[i] == 'i' ||
            A[i] == 'o' ||
            A[i] == 'u')
                if (!isPrime(i+1)) return "NO";
    }
    return "YES";
}
```

### Mine
```cpp
string Solution::solve(string A) {
    
    int n = A.size();
    vector<bool> isPrime(n+1, true);
    isPrime[0] = isPrime[1] = false;
    for (int p=2; p*p<=n; p++)
        if (isPrime[p])
            for (int i=p*p; i<=n; i+=p) 
                isPrime[i] = false;
                
    for (int i=0; i<n; i++)
        if ((A[i]=='a' || A[i]=='e' || A[i]=='i' || A[i]=='o' || A[i]=='u') && !isPrime[i+1])
            return "NO";
        
    return "YES";
}

```
