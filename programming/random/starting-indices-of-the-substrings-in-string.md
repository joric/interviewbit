# Starting indices of the substrings in string

https://www.interviewbit.com/problems/starting-indices-of-the-substrings-in-string/

Given a string A and list of words B(words in B are of same length).

Find and return the starting indices of the substrings in string A, which contains all the words present in list B.

### Note

The order of words of list B appearing inside string S does not matter.

Words inside the list B can repeat.

### Input Format

The arguments given are string A and a list of string B.

### Output Format

Return the starting indices of the substrings in string A, which contains all the words present in list B.

### Constraints

```
1 <= length of the string A <= 4000
1 <= total length of list B if all the words are concatenated <= 4000
```

### For Example

```
Input 1:
    A = "barfoothefoobarman" 
    B = ["foo", "bar"] 
Output 1:
     [0, 9]

Input 2:
    S = "catbatatecatatebat"
    L = ["cat", "ate", "bat"] 
Output 2:
     [0, 3, 9]
```

## Solution
### Editorial
```cpp
vector<int> Solution::solve(string A, vector<string> &B){
    int size = B.size(), window = size * B[0].size();
    unordered_map<string, int> mp;
    for(string s: B){
        mp[s]++;
    }
    /// I can use sliding window, of size window
    vector<int> res;
    for(int i=0;i<=(A.size()-window);i++){
        unordered_map<string, int> mp2;
        bool brek = false; //// spelling xD
        string curr="";
        for(int k=i;k<window+i;k++){
            curr += A[k];
            if(curr.size() == B[0].size()){
                if(!mp.count(curr)){
                    brek=true;
                    curr = "";
                    break;
                }
                mp2[curr]++;
                curr = "";
            }
        }
        
        if(!brek){
            bool brek2=false;
            for(auto it=mp.begin();it!=mp.end();it++){
                if(it->second != mp2[it->first]){
                    brek2=true;
                    break;
                }
            }
            
            if(!brek2){
                res.push_back(i);
            }
        }
    }
    return res;
}
```

### Fastest
```cpp
typedef vector<int> vi;
#define pb push_back
vector<int> Solution::solve(string A, vector<string> &B) {
    int n= B.size();
    int m = B[0].size();
    int M = A.size();
    vi ans;
    if(m==0||n==0||M==0)return ans;
    unordered_map<string,pair<int,int>> s;
    for(auto i:B){
        if(s.find(i)==s.end())  
            s.insert({i,{1,1}});
        else{
            auto it = s.find(i);
            it->second.first++;
            it->second.second++;
        }
    }
    //  for (auto it=s.begin(); it!=s.end(); ++it)
        // std::cout << it->first << " => " << it->second.first << ' ' << it->second.second << '\n';
        // cout <<"n " << n <<  endl;
    
    for(int i= 0;i<M;i++){
        // cout << "i " << i << endl;
       string temp = A.substr(i,m);
       int j = i;
    //   cout << temp  << endl;
      bool T = false;
       while(j<M and (s.find(temp) != s.end()) and s.find(temp)->second.first>0){
           T=true;
           s.find(temp)->second.first--;
           j=j+m;
           temp = A.substr(j,m);
        //   cout << "j " << j  << endl;
           
       }
       bool flag = true;
       for (auto it=s.begin(); it!=s.end(); ++it)
            if(it->second.first != 0){
                flag = false;
                break;
            }
            
        if(flag==true and T == true){
            ans.pb(i);
            
        }
        for (auto it=s.begin(); it!=s.end(); ++it)
            it->second.first = it->second.second;
            
    // cout << "i " << i << endl;
    }
    return ans;
}```

### Lightweight
```cpp

vector<int> findSubstringIndices(string &S,  vector<string>& L) {

    // Number of a characters of a word in list L.
    int size_word = L[0].size();

    // Number of words present inside list L.
    int word_count = L.size();

    // Total characters present in list L.
    int size_L = size_word * word_count;

    // Resultant vector which stores indices.
    vector<int> res;

    // If the total number of characters in list L
    // is more than length of string S itself.
    if (size_L > S.size()) {
        return res;
        }

    // Map stores the words present in list L
    // against it's occurrences inside list L
    unordered_map<string, int> hash_map;

    for (int i = 0; i < word_count; i++)
        hash_map[L[i]]++;

    for (int i = 0; i <= S.size() - size_L; i++) {
        unordered_map<string, int> temp_hash_map(hash_map);

        int j = i;

        // Traverse the substring
        while (j < i + size_L) {

            // Extract the word
            string word = S.substr(j, size_word);

            // If word not found simply break.
            if (hash_map.find(word) == hash_map.end())
                break;

            // Else decrement the count of word from hash_map
            else
                temp_hash_map[word]--;

            j += size_word;
        }

        int count = 0;
        for (auto itr = temp_hash_map.begin();
             itr != temp_hash_map.end(); itr++)
            if (itr->second > 0)
                count++;

        // Store the starting index of that substring
        if (count == 0)
            res.push_back(i);
    }
 // for(auto &it:res)cout<<it<<" ";
  //cout<<"\n";
    return res;
}

vector<int> Solution::solve(string A, vector<string> &B) {
    return findSubstringIndices(A,B);
}
```
