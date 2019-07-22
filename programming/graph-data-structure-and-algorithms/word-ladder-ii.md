# Word Ladder II
https://www.interviewbit.com/problems/word-ladder-ii/

Given two words (start and end), and a dictionary, find the shortest transformation sequence from start to end, such that:

Only one letter can be changed at a time
Each intermediate word must exist in the dictionary
If there are multiple such sequence of shortest length, return all of them. Refer to the example for more details.

Example :

Given:
```
start = "hit"
end = "cog"
dict = ["hot","dot","dog","lot","log"]
```
Return
```
  [
    ["hit","hot","dot","dog","cog"],
    ["hit","hot","lot","log","cog"]
  ]
```
Note:

All words have the same length.

All words contain only lowercase alphabetic characters.

## Hint 1

Think in terms of a graph.

When can you do the transition from one word to another ? Does it mean it can indicate a graph edge between those 2 words ? How can this graph help you to achieve the purpose?

## Solution Approach

This is a classic shortest path problem.

Think in terms of a graph. You basically need to add an edge between two words which can be converted into one another. Resulting graph will be unweighted and undirected.
Which graph traversal algorithm can now help you in computing the shortest path in undirected, unweighted graph?

Once you know about the shortest path, how do we construct all shortest paths ? 
When will a node x be a parent of node Y ?


## Solution
### Editorial

```cpp
void constructPaths(string start, string &end, unordered_map<string, vector<string>> &parents, vector<string> &current, vector<vector<string>> &answer) {
    if (start == end) {
        answer.push_back(current);
        return;
    }
    for (int i = 0; i < parents[start].size(); i++) {
        current.push_back(parents[start][i]);
        constructPaths(parents[start][i], end, parents, current, answer);
        current.pop_back();
    }
    return;
}

vector<vector<string>> Solution::findLadders(string start, string end, vector<string> &dictV) {
    unordered_set<string> dict(dictV.begin(), dictV.end());
    vector<vector<string>> answer;
    unordered_map<string, int> distance; // store the distance from start to the current word
    queue<string> q;                     // FIFO for bfs purpose
    unordered_map<string, vector<string>> parents;
    swap(start, end); // We do this because we construct the path later from end to start
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
                if (dict.find(newWord) != dict.end()) {
                    if (distance.find(newWord) == distance.end()) { // seen for the first time
                        distance[newWord] = distance[word] + 1;
                        q.push(newWord);
                        parents[newWord].push_back(word);
                    } else if (distance[newWord] == distance[word] + 1) {
                        parents[newWord].push_back(word);
                    }
                }
            }
        }
    }
    if (distance.find(end) == distance.end()) return answer; // not found
    // backtrack and construct all possible paths now that we know possible optimal parents.
    vector<string> current;
    current.push_back(end);
    constructPaths(end, start, parents, current, answer);
    return answer;
}

```

### BFS-only

```cpp
vector<vector<string> > Solution::findLadders(string start, string end, vector<string> &d) {
   queue<vector<string> >paths;
   vector<vector<string> >ans;
   paths.push({start});
   if(start==end){
       ans.push_back({start});
       return ans;
   }
   unordered_set<string>dict;
   for(int i=0; i<d.size(); i++){
       dict.insert(d[i]);
   }
   int level = 1;
   int min_level = INT_MAX;
   unordered_set<string>visited;
   while(!paths.empty()){
       vector<string> path = paths.front();
       paths.pop();
       if(path.size()>level){
           for(auto w: visited){
               dict.erase(w);
           }
           visited.clear();
           if(path.size()>min_level)
           break;
           else
           level = path.size();
       }
       string last = path.back();
       for(int i=0; i<last.length(); i++){
           string news = last;
           for(int j='a'; j<='z'; j++){
               news[i] = j;
               if(dict.find(news)!=dict.end()){
                   vector<string>newpath = path;
                   newpath.push_back(news);
                   visited.insert(news);
                   if(news==end){
                       min_level = level;
                       ans.push_back(newpath);
                   }
                   else{
                       paths.push(newpath);
                   }
               }
           }
       }
   }
   return ans;
}
```

## Asked in
* Google
* Microsoft
* Ebay

