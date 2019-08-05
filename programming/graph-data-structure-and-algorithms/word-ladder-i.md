# Word Ladder I

https://www.interviewbit.com/problems/word-ladder-i/

Given two words (start and end), and a dictionary, find the length of shortest transformation
sequence from start to end, such that:

1. You must change exactly one character in every transformation
2. Each intermediate word must exist in the dictionary

### Example

```
Given:
start = "hit"
end = "cog"
dict = ["hot","dot","dog","lot","log"]
As one shortest transformation is "hit" -> "hot" -> "dot" -> "dog" -> "cog", return its length 5.
```

Note that we account for the length of the transformation path instead of the number of transformation itself.

1. Return 0 if there is no such transformation sequence.
2. All words have the same length.
3. All words contain only lowercase alphabetic characters.

## Hint 1

Think in terms of a graph.

When can you do the transition from one word to another ? Does it mean it can indicate a graph edge between those 2 words ? How can this graph help you to achieve the purpose?

## Solution Approach

This is a classic shortest path problem.

Think in terms of a graph. You basically need to add an edge between two words which can be converted into one another. Resulting graph will be unweighted and undirected.

Which graph traversal algorithm can now help you in computing the shortest path in undirected, unweighted graph?

## Solution

### Editorial
```cpp
int Solution::ladderLength(string start, string end, vector<string> &dictV) {
    unordered_set<string> dict(dictV.begin(), dictV.end());
    unordered_map<string, int> distance; // store the distance from start to the current word
    queue<string> q; // FIFO for bfs purpose
    distance[start] = 1;
    q.push(start);
    while (!q.empty()) {
        string word = q.front(); 
        q.pop();
        if (word == end) break;
        for (int i = 0; i < word.size(); i++) {
            for (int j = 0; j < 26; j++) {
                string newWord = word;
                newWord[i] = 'a' + j;
                if (dict.find(newWord) != dict.end() && distance.find(newWord) == distance.end()) {
                    distance[newWord] = distance[word] + 1;
                    q.push(newWord);
                }
            }
        }
    }
    if (distance.find(end) == distance.end()) return 0; // not found
    return distance[end];
}
```
### Fastest
```cpp
bool dist(string a, string b){
    int count=0;
    for(int i=0;i<a.length();i++){
        if(a[i]!=b[i]) count++; 
    }
    
    if(count!=1) 
    return false;
    else 
    return true;
}

int Solution::ladderLength(string start, string end, vector<string> &dictV) {
    
    if(start==end) return 1;
    
    if(dist(start,end)) return 2;
    
    int n=dictV.size();
    vector<bool> visit(n,false);
    queue<pair<int,string> > q;
    
    q.push(make_pair(1,start));
    while(!q.empty()){
        
        pair<int,string> pos=q.front();
        q.pop();
        int level=pos.first;
        if(dist(pos.second,end)) return pos.first+1;
        level++;
        for(int i=0;i<n;i++){
            if(!visit[i] && dist(dictV[i],pos.second)){
                //cout<<level<<" ";
                visit[i]=true;
                q.push(make_pair(level,dictV[i]));
            }
        }
    }
    return 0;
}
```
### Lightweight
```cpp
vector<string> fun(string &s,vector<string>& wordDict)
{
    int i,j,k;
    char t;
    vector<string> result;
    for(i=0;i<s.length();i++)
    {
        char c=s[i];
        for(j=0;j<26;j++)
        {
           t='a'+j;
           if(s[i]!=t)
            {
                s[i]=t;
                for(k=0;k<wordDict.size();k++)
                {
                    if(wordDict[k]==s)
                    {
                        result.push_back(s);
                        wordDict[k]="*";
                        break;
                    }
                }
                
                s[i]=c;
            }
        }
    }
    
    return result;
}
int Solution::ladderLength(string beginWord, string endWord, vector<string> &wordDict) {
    
        queue<pair<string,int>  > q;
        string temp;
        wordDict.push_back(endWord);
        int len,i;
        q.push(make_pair(beginWord,1));
        
        while(!q.empty())
        {
            
            temp=q.front().first;
            len=q.front().second;
            if(temp==endWord)
                return len;
            q.pop();
            vector<string> n=fun(temp,wordDict);
            for(i=0;i<n.size();i++)
                q.push(make_pair(n[i],len+1));
        
        }
        
        return 0;
}
```
## Asked in
* Google
* Microsoft
* Ebay

