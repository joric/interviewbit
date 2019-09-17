# First non-repeating character in a stream of characters

https://www.interviewbit.com/problems/first-non-repeating-character-in-a-stream-of-characters/

Given a string A denoting a stream of lowercase alphabets. You have to make new string B. B is formed such that
we have to find first non-repeating character each time a character is inserted to the stream and append it at the end to B.
if no non-repeating character is found then append ’#’ at the end of B.

### Input Format

The only argument given is string A.

### Output Format

Return a string B after processing the stream of lowercase alphabets A.

### Constraints

1 <= length of the string <= 100000

### For Example
```
Input 1:
    A = "abadbc"
Output 1:
    "aabbdd"
    
    Explanation:
    "a"      -   first non repeating character 'a'
    "ab"     -   first non repeating character 'a'
    "aba"    -   first non repeating character 'b'
    "abad"   -   first non repeating character 'b'
    "abadb"  -   first non repeating character 'd'
    "abadbc" -   first non repeating character 'd'

Input 2:
    A = "abcabc"
Output 2:
    "aaabc#"
    
    Explanation
    "a"      -   first non repeating character 'a'
    "ab"     -   first non repeating character 'a'
    "abc"    -   first non repeating character 'a'
    "abca"   -   first non repeating character 'b'
    "abcab"  -   first non repeating character 'c'
    "abcabc" -   no non repeating character so '#'
```

## Solution
### Editorial
```cpp
string Solution::solve(string A) {
    unordered_map<char, int> mp;
    queue<char> q;
    string ans;
    for (auto c : A)
    {
        mp[c]++;
        q.push(c);
        while (!q.empty() && mp[q.front()] > 1)   q.pop();
        if (!q.empty())                         ans.push_back(q.front());
        else                                    ans.push_back('#');
    }
    return ans;
}
```

### Fastest
```cpp
string Solution::solve(string A) 
{
    string res;
    vector<int> visited(26,0);
    queue<int> q; 
    
    for(int i=0;i<A.size();i++)
    {
       
        if(visited[A[i]-'a']==0)
        {
            visited[A[i]-'a']=1;
            q.push(A[i]);
        }else
        {
            visited[A[i]-'a']=2;
        }
        
        while(!q.empty()&&visited[q.front()-'a']==2)
            q.pop();
            
        if(!q.empty())
        {
           
            
            res.push_back(q.front());
        }else
        {
            res.push_back('#');
        }
    }
    
    return res;
}
```

### Lightweigth
```cpp
string Solution::solve(string A)
{
    string s=A;
    int arr[26];
    for(int i=0;i<26;i++)
    arr[i]=0;
    string ans=A;
    int  ind[26];
    for(int i=0;i<26;i++)
    ind[i]=50000000;
    for(int i=0;i<s.length();i++)
    {
        arr[s[i]-'a']++;
        int flag=0;
        ind[s[i]-'a']=min(ind[s[i]-'a'],i);
        int curr=5000000;
        for(int j=0;j<26;j++)
        {
            if(arr[j]==1)
            {
                if(curr>ind[j])
                {
                    ans[i]=(char)(j+'a');
                    flag=1;
                    curr=ind[j];
                }
            }
        }
        if(!flag)
        ans[i]='#';
    }
    return ans;
}
```

### Mine
```cpp
struct node {
    char a;
    struct node *next, *prev;
};

void appendNode(struct node **head_ref, struct node **tail_ref, char x) {
    struct node *temp = new node;
    temp->a = x;
    temp->prev = temp->next = 0;

    if (*head_ref == 0) {
        *head_ref = *tail_ref = temp;
        return;
    }

    (*tail_ref)->next = temp;
    temp->prev = *tail_ref;
    *tail_ref = temp;
}

void removeNode(struct node **head_ref, struct node **tail_ref,
                struct node *temp) {
    if (*head_ref == 0)
        return;

    if (*head_ref == temp)
        *head_ref = (*head_ref)->next;
    if (*tail_ref == temp)
        *tail_ref = (*tail_ref)->prev;
    if (temp->next != 0)
        temp->next->prev = temp->prev;
    if (temp->prev != 0)
        temp->prev->next = temp->next;

    delete (temp);
}

#define MAX_CHAR 256
string Solution::solve(string stream) {
    string res;
    struct node *inDLL[MAX_CHAR];
    bool repeated[MAX_CHAR];

    struct node *head = 0, *tail = 0;
    for (int i = 0; i < MAX_CHAR; i++) {
        inDLL[i] = 0;
        repeated[i] = false;
    }

    for (int i = 0; stream[i]; i++) {
        char x = stream[i];
        if (!repeated[x]) {
            if (inDLL[x] == 0) {
                appendNode(&head, &tail, stream[i]);
                inDLL[x] = tail;
            } else {
                removeNode(&head, &tail, inDLL[x]);
                inDLL[x] = 0;
                repeated[x] = true;
            }
        }
        res += head ? head->a : '#';
    }
    return res;
}
```

### Another mine
```cpp
string Solution::solve(string s) {
    unordered_map<char, int> m;
    queue<char> q;
    string res;
    for (char c:s) {
        m[c]++;
        q.push(c);
        while (!q.empty() && m[q.front()] > 1) q.pop();
        res.push_back(q.empty() ? '#' : q.front());
    }
    return res;
}
```
