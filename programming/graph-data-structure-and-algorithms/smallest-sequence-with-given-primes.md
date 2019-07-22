# Smallest sequence with given Primes

https://www.interviewbit.com/problems/smallest-sequence-with-given-primes/

Given three prime number(p1, p2, p3) and an integer k. Find the first(smallest) k integers which have only p1, p2, p3 or a combination of them as their prime factors.

Example:

Input : 

Prime numbers : [2,3,5] 

k : 5

If primes are given as p1=2, p2=3 and p3=5 and k is given as 5, then the sequence of first 5 integers will be: 

Output: 

{2,3,4,5,6}

Explanation :

4 = p1 * p1 ( 2 * 2 )

6 = p1 * p2 ( 2 * 3 )

Note: The sequence should be sorted in ascending order

# Hint 1

Naive solution : We are interested in only those numbers whose prime factorization contains no other prime number other than p1, p2 and p3. So we can simply iterate all numbers and can check their prime factorization.

Better solution : Can we incrementally construct new numbers starting from the lowest prime p1 ? Would this become similar to Breadth first search / Dijkstra ?

# Solution Approach

The naive solution will be to check prime factorization of every natural number incrementally till k numbers are found. However, that will be too slow.

As mentioned in the previous hint, this problem can be addressed as a modified BFS / Dijkstra. We push p1, p2 and p3 to a min heap. 
Every time, we repeat the following process till we find k numbers :
```
 - M = Pop out the min element in the min heap. 
 - Add M to the final answer. 
 - Push M * p1, M * p2, M * p3 to the min heap if they are not already present in the heap ( We can use a hash map to track this ) 
```
However, this is O( k * log k ). 
Can we do better than this ?

It turns out we can. 
We use the fact that there are only 3 possibilities of getting to a new number : Multiply by p1 or p2 or p3.

For each of p1, p2 and p3, we maintain the minimum number in our set which has not been multiplied with the corresponding prime number yet. 
So, at a time we will have 3 numbers to compare. 
The corresponding approach would look something like the following :

```
initialSet = [p1, p2, p3] 
indexInFinalSet = [0, 0, 0]

for i = 1 to k 
  M = get min from initialSet. 
  add M to the finalAnswer if last element in finalAnswer != M
  if M corresponds to p1 ( or in other words M = initialSet[0] )
    initialSet[0] = finalAnswer[indexInFinalSet[0]] * p1
    indexInFinalSet[0] += 1
  else if M corresponds to p2 ( or in other words M = initialSet[1] )
    initialSet[1] = finalAnswer[indexInFinalSet[1]] * p1
    indexInFinalSet[1] += 1
  else 
    # Similar steps for p3. 
end
```

## Solution

### Editorial

```cpp
vector<int> Solution::solve(int A, int B, int C, int D) {
    vector<int> res;
    res.push_back(1);
    int i = 0, x = 0, y = 0, z = 0;
    while(i < D)  {
        int tmp = min(A*res[x], min(B*res[y], C*res[z]));
        res.push_back(tmp);
        ++i;
        if(tmp == A*res[x]) ++x;
        if(tmp == B*res[y]) ++y;
        if(tmp == C*res[z]) ++z;
    }
    res.erase(res.begin());
    return res;
}

```

### Fastest

```cpp
vector<int> Solution::solve(int A, int B, int C, int D) {
    vector<int> res;
    res.push_back(1);
    int iA = 0,iB = 0,iC = 0;
    
    for(int i = 0 ;i<D;i++){
        int nextA = res[iA] * A;
        int nextB = res[iB] * B;
        int nextC = res[iC] * C;
        int nextN = min(nextA,min(nextB,nextC));
        
        if(nextN == nextA)
        iA++;
        
        if(nextN == nextB)
        iB++;
        
        if(nextN == nextC)
        iC++;
        
        res.push_back(nextN);
    }
    
   // res.pop_front();
    vector<int> v;
   for(int i = 0;i<D;i++){
       v.push_back(res[i+1]);
   }
   
   return v;
}

```

### Lightweight

```cpp
class Compare {
 public:
  bool operator()(pair<int,int> a, pair<int, int> b) {
    return a.first > b.first;
  }
};

vector<int> Solution::solve(int A, int B, int C, int K) {
  vector<int> result;
  vector<int> tmp = {A, B, C};
  sort(tmp.begin(), tmp.end());
  A = tmp[0];
  B = tmp[1];
  C = tmp[2];
  priority_queue<pair<int, int>, vector<pair<int, int>>, Compare> minHeap;
  minHeap.push(make_pair(A, A));
  minHeap.push(make_pair(B, B));
  minHeap.push(make_pair(C, C));
  while (result.size() < K && !minHeap.empty()) {
    if (result.size() > 0) {
      while (result[result.size() - 1] == minHeap.top().first) {
        minHeap.pop();
      }
    }
    pair<int, int> tmpMin = minHeap.top();
    minHeap.pop();
    result.push_back(tmpMin.first);
    if (tmpMin.second == A) {
      minHeap.push(make_pair(tmpMin.first * A, A));
      minHeap.push(make_pair(tmpMin.first * B, B));
      minHeap.push(make_pair(tmpMin.first * C, C));
    } else if (tmpMin.second == B) {
      minHeap.push(make_pair(tmpMin.first * B, B));
      minHeap.push(make_pair(tmpMin.first * C, C));
    } else if (tmpMin.second == C) {
      minHeap.push(make_pair(tmpMin.first * C, C));
    }
  }

  return result;
}
```

### My

```cpp
vector<int> Solution::solve(int A, int B, int C, int D)
{
    set<int> s;
    swap(A,D);
    int ctr = 0;
    vector<int> ans;
    s.insert(B);
    s.insert(C);
    s.insert(D);
    while(ctr<A)
    {
        auto i = s.begin();
        ans.push_back(*i);
    
        s.insert((*i)*B);
        s.insert((*i)*C);
        s.insert((*i)*D);
        s.erase(i);
        ctr++;
    }
    return ans;
}
```
