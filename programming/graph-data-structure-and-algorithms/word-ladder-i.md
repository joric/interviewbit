# Word Ladder I

https://www.interviewbit.com/problems/word-ladder-i/

Given two words (start and end), and a dictionary, find the length of shortest transformation sequence from start to end, such that:

You must change exactly one character in every transformation
Each intermediate word must exist in the dictionary
Example :

Given:
```
start = "hit"
end = "cog"
dict = ["hot","dot","dog","lot","log"]
```

As one shortest transformation is `"hit" -> "hot" -> "dot" -> "dog" -> "cog"`, return its length 5.

Note that we account for the length of the transformation path instead of the number of transformation itself.

Note:

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
int Solution:: ladderLength(string start, string end, vector<string> &dictV) {
    unordered_set<string> dict(dictV.begin(), dictV.end());
    unordered_map<string, int> distance; // store the distancetance from start to the current word
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


