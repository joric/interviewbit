# Parity Queries

https://www.interviewbit.com/problems/parity-queries/

Given an empty multiset of integers initially and N queries to perform on it.

* +X — add non-negative integer X to the multiset. Note, that there may be many occurrences of the same integer.

* -X — delete a single occurrence of non-negative integer X from the multiset. It’s guaranteed, that there is at least one X in the multiset.

* ?S — count the number of integers in the multiset (with repetitions) that match some pattern S consisting of 0 and 1.

In the pattern, 0 stands for the even digits, while 1 stands for the odd.
Integer X matches the pattern S, if the parity of the i’th from the right digit in decimal notation matches the i’th from the right digit of the pattern.

If the pattern is shorter than this integer, it’s supplemented with 0’s from the left.
Similarly, if the integer is shorter than the pattern its decimal notation is supplemented with the 0’s from the left.

For example, if the pattern is S = 010, than integers 92, 2212, 50 and 414 match the pattern, while integers
3, 110, 25 and 1030 do not.

Given an array of characters A and an array of Strings B (representing the value of X for each query)
of size N each, perform the queries and return the result for every third query in the form of an array of integers.


### Input Format
```
The first argument given is the character array A.
The second argument given is the String array B.
```

### Output Format

Return the answer for every third query in the form of an array of integers.

### Constraints
```
1 <= N <= 100000
A[i] = {'+', '-', '?'}
0 <= B[i] <= 10^18  (given in the form of string)
```
### For Example
```
Input 1:
    A = ['+', '+', '-', '?']
    B = [200, 200, 200, 0]
Output 1:
    [1]

Input 2:
    A = ['+', '+', '?', '+', '-', '?']
    B = [1, 241, 1, 361, 241, 101]
Output 2:
    [2, 1]
```

## Hint 1

Try converting every digit of the number into 0 (if it is even) and 1 (if it is odd).
Then the number will appear to be in base 2 representation.
Pattern is also given in base 2 representation.

Use this to store / increment / decrement the count of every number and display when required.

## Solution
### Editorial
```cpp
//convert every string to binary number
long long int convert(string s){
    long long int ans = 0;
    int n = s.length(),j=0;
    for(int i=n-1;i>=0;i--){
        //if ith number is odd then set (n-1-i)th bit in ans
        if((s[i]-'0')&1){
            j = n-1-i;
            ans = ans|(1<<j);
            
        }
    }
    return ans;
}
vector<int> Solution::solve(vector<char> &A, vector<string> &B) {
    int n = A.size();
    vector<int> ans;
    unordered_map<long long int,int> m;
    for(int i=0;i<n;i++){
        long long int x = convert(B[i]);
        if(A[i]=='+'){
            m[x]++;
        }
        else if(A[i]=='-'){
            m[x]--;
        }
        else{
            if(m.find(x)==m.end()){
                ans.push_back(0);
            }
            else{
                ans.push_back(m[x]);
            }
        }
    }
    //cout<<endl;
    return ans;
}
```

### Fastest
```cpp

const int maxn=262144+5;
int num[maxn];

vector<int> Solution::solve(vector<char> &A, vector<string> &B)
{
    
    memset(num,0,sizeof(num));
    
    int t = A.size();
    
    int j = 0;
    
    vector<int> ans;
    
    while(t--)
    {
        long long int num_1 = stoll(B[j]);
        //getchar();
        //scanf("%c%I64d",&c,&num_1);
        long long int num_2=0;
        for(int i=0;i<18;i++)
        {
            num_2=num_2*2+num_1%2;
            num_1=num_1/10;
        }
        if(A[j]=='+')
            num[num_2]++;
        else if(A[j]=='-')
            num[num_2]--;
        else if(A[j]=='?')
            ans.push_back(num[num_2]);
        
        j++;
        
    }
    
    return ans;
    
}
```
### Lightweigth
```cpp


const int maxn=262144+5;
int num[maxn];

vector<int> Solution::solve(vector<char> &A, vector<string> &B)
{
    
    memset(num,0,sizeof(num));
    
    int t = A.size();
    
    int j = 0;
    
    vector<int> ans;
    
    while(t--)
    {
        long long int num_1 = stoll(B[j]);
        long long int num_2=0;
        for(int i=0;i<18;i++)
        {
            num_2=num_2*2+num_1%2;
            num_1=num_1/10;
        }
        if(A[j]=='+')
            num[num_2]++;
        else if(A[j]=='-')
            num[num_2]--;
        else if(A[j]=='?')
            ans.push_back(num[num_2]);
        
        j++;
        
    }
    
    return ans;
    
}
```

