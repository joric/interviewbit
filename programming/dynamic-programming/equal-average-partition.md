# Equal Average Partition

https://www.interviewbit.com/problems/equal-average-partition/

Given an array with non negative numbers, divide the array into two parts such that the average of both the parts is equal. 
Return both parts (If exist).

If there is no solution. return an empty list.

Example:

```
Input:
[1 7 15 29 11 9]

Output:
[9 15] [1 7 11 29]
```

The average of part is (15+9)/2 = 12,

average of second part elements is (1 + 7 + 11 + 29) / 4 = 12

### Note 1
If a solution exists, you should return a list of exactly 2 lists of integers A and B which follow the following condition :
numElements in A <= numElements in B

If numElements in A = numElements in B, then A is lexicographically smaller than B ( https://en.wikipedia.org/wiki/Lexicographical_order )

### Note 2
If multiple solutions exist, return the solution where length(A) is minimum. If there is still a tie, return the one where A is lexicographically smallest. NOTE 3: Array will contain only non negative numbers. 

## Hint 1

Lets try to simplify the problem.

Lets assume the two sets are set1 and set2.

```
Assume sum of set1 = Sum_of_Set1, with size = size_of_set1. 
Assume sum of set2 = Sum_of_Set2, with size = size_of_set2

SUM_of_Set1 / size_of_set1 = SUM_of_Set2 / size_of_set2
SUM_of_Set1 = SUM_of_Set2 * (size_of_set1 / size_of_set2)

total_sum = Sum_of_Set1 + Sum_of_Set2
AND size_of_set2 = total_size - size_of_set1

Sum_of_Set1 = (total_sum - Sum_of_Set1) * (size_of_set1 / (total_size - size_of_set1))
OR on simplifying,
  
total_sum / Sum_of_Set1 - 1 = (total_size - size_of_set1) / size_of_set1
total_sum / Sum_of_Set1 = total_size / size_of_set1
Sum_of_Set1 / size_of_set1 = total_sum / total_size
```

Note that you need the solution with minimum size_of_set1 if multiple solutions exist. 

So, just iterate on size_of_set1. 

Based on size_of_set1, you can determine the value of Sum_of_Set1. 

Now, the problem reduces to

Can I select size_of_set1 values from the array whose sum is Sum_of_Set1 ? 

## Solution Approach

**Continue from hint **

In previous hint, we explored how we can break down the given problem into a much simpler problem

Can I select current_size values from the array whose sum is current_sum ? 

Lets define our function as 

```
isPossible(ind, current_sum, current_size)
```

which returns true if it is possible to use elements 
with index >= ind to construct a set of size current_size whose sum is current_sum.

```
isPossible(ind, current_sum, current_size :            |
                                                       |
                                                       |  isPossible(ind + 1, current_sum, current_size)  [ Do not include current element ]
                                Or(|) Logical operator |
                                                       |
                                                       | 
                                                       |  
                                                       |  isPossible(ind + 1, current_sum - value_at(ind), current_size - 1)
                                                       |

```
Can you memoize values to reduce the time complexity of the above recursive function ?


## Solution

### Editorial
```cpp
vector<vector<vector<bool>>> dp;
vector<int> res;
vector<int> original;
int total_size;

bool possible(int index, int sum, int sz) {
    if (sz == 0) return (sum == 0);
    if (index >= total_size) return false;

    if (dp[index][sum][sz] == false) return false;

    if (sum >= original[index]) {
        res.push_back(original[index]);
        if (possible(index + 1, sum - original[index], sz - 1)) return true;
        res.pop_back();
    }

    if (possible(index + 1, sum, sz)) return true;

    return dp[index][sum][sz] = false;
}

vector<vector<int>> Solution::avgset(vector<int> &Vec) {
    sort(Vec.begin(), Vec.end());
    original.clear();
    original = Vec;
    dp.clear();
    res.clear();

    int total_sum = 0;
    total_size = Vec.size();

    for (int i = 0; i < total_size; ++i)
        total_sum += Vec[i];

    dp.resize(original.size(), vector<vector<bool>>(total_sum + 1, vector<bool>(total_size, true)));

    // We need to minimize size_of_set1. So, lets search for the first size_of_set1 which is possible.
    for (int i = 1; i < total_size; i++) {
        // Sum_of_Set1 has to be an integer
        if ((total_sum * i) % total_size != 0) continue;
        int Sum_of_Set1 = (total_sum * i) / total_size;
        if (possible(0, Sum_of_Set1, i)) {

            // Ok. Lets find out the elements in Vec, not in res, and return the result.
            int ptr1 = 0, ptr2 = 0;
            vector<int> res1 = res;
            vector<int> res2;
            while (ptr1 < Vec.size() || ptr2 < res.size()) {
                if (ptr2 < res.size() && res[ptr2] == Vec[ptr1]) {
                    ptr1++;
                    ptr2++;
                    continue;
                }
                res2.push_back(Vec[ptr1]);
                ptr1++;
            }

            vector<vector<int>> ans;
            ans.push_back(res1);
            ans.push_back(res2);
            return ans;
        }
    }
    // Not possible.
    vector<vector<int>> ans;
    return ans;
}

```

### Fastest
```cpp
using VecSolutionsForSums = std::vector<bool>;
using VecSolutionsForSizeAndSums = std::vector<VecSolutionsForSums>;
using Opt = std::vector<VecSolutionsForSizeAndSums>;

static bool
get_set_with_size_and_sum(const std::vector<int> &available,
                          std::size_t i, std::size_t size, int sum, Opt &opt, std::vector<int> &set) {
    const auto n = available.size();

    if (size == 0) {
        return sum == 0;
    }

    if (i >= n) {
        return false;
    }

    if (size > n) {
        return false;
    }

    // Return cached results:
    auto &opti = opt[i][size];
    const auto c = opti[sum];
    if (!c) {
        return c;
    }

    // Try with the current item:
    const auto v = available[i];
    if (sum >= v) {
        set.emplace_back(v);
        if (get_set_with_size_and_sum(available,
                                      i + 1, size - 1, sum - v, opt, set)) {
            //assert(!(set.size() > size));
            return true;
        }
        set.pop_back();
    }

    // Try without using the current item:
    if (get_set_with_size_and_sum(
            available, i + 1, size, sum, opt, set)) {
        return true;
    }
    //assert(!(set.size() > size));

    return opti[sum] = false;
}

static std::vector<int>
get_set2(const std::vector<int> &available,
         const std::vector<int> &set1) {
    std::vector<int> result;
    const auto n = available.size();
    const auto s1n = set1.size();

    result.reserve(available.size() - set1.size());

    std::size_t j = 0;
    for (std::size_t i = 0; i < n; ++i) {
        const auto v = available[i];
        if (j < s1n && v == set1[j]) {
            ++j;
            continue;
        }

        result.emplace_back(v);
    }

    return result;
}

vector<vector<int>> Solution::avgset(vector<int> &A) {
    const auto n = A.size();
    if (n < 2) {
        return {};
    }

    const int total = std::accumulate(std::begin(A), std::end(A), 0);

    // So that a sorted vector of indices
    // represents a sorted vector of values:
    std::sort(std::begin(A), std::end(A));

    Opt opt(n + 1, VecSolutionsForSizeAndSums(n + 1, VecSolutionsForSums(total, true)));

    const auto half = ((n + 1) / 2);
    for (auto i = 1u; i <= half; ++i) {
        // The sum must be an integer:
        // Instead of just checking average * i,
        // which is total/n * i, we check total * i / n,
        // so we can do the % check last.
        const auto totalmi = total * i;
        if (totalmi % n != 0) {
            continue;
        }

        const auto sum = (total * i) / n;

        std::vector<int> set1;
        set1.reserve(n);
        if (!get_set_with_size_and_sum(A, 0, i, sum, opt, set1)) {
            continue;
        }

        const auto set2 = get_set2(A, set1);
        if (!set2.empty()) {

            return { set1, set2 };
        }
    }

    return {};
}

```

### Lightweight
```cpp
int getHash(int s, int z, int ii) {
    return s - z + ii;
}

bool isInCache(int nrRemainingElements, int currentIdx, int elementsSum,
               vector<vector<vector<bool>>> &cache) {
    if (cache.size() <= nrRemainingElements) {
        return false;
    }
    if (cache[nrRemainingElements].size() <= currentIdx) {
        return false;
    }
    if (cache[nrRemainingElements][currentIdx].size() <= elementsSum) {
        return false;
    }
    return cache[nrRemainingElements][currentIdx][elementsSum];
}

void addToCache(int nrRemainingElements, int currentIdx, int elementsSum, vector<vector<vector<bool>>> &cache) {
    if (cache.size() <= nrRemainingElements) {
        cache.resize(nrRemainingElements + 1);
    }
    if (cache[nrRemainingElements].size() <= currentIdx) {
        cache[nrRemainingElements].resize(currentIdx + 1);
    }
    if (cache[nrRemainingElements][currentIdx].size() <= elementsSum) {
        cache[nrRemainingElements][currentIdx].resize(elementsSum + 1);
    }
    cache[nrRemainingElements][currentIdx][elementsSum] = true;
}

vector<vector<int>> pick(int elementsSum, int nrRemainingElements, int currentIdx, vector<int> &A,
                         vector<vector<vector<bool>>> &cache) {
    vector<vector<int>> result;

    if (nrRemainingElements == 0 || elementsSum < 0 || currentIdx >= A.size() || currentIdx > A.size() - nrRemainingElements) {
        //addToCache(nrRemainingElements, currentIdx, elementsSum, cache);
        return result;
    }

    int key = getHash(elementsSum, nrRemainingElements, currentIdx);
    if (isInCache(nrRemainingElements, currentIdx, elementsSum, cache)) {
        return result;
    }

    if (nrRemainingElements == 1 && currentIdx == A.size() - 1 && A[currentIdx] != elementsSum) {
        addToCache(nrRemainingElements, currentIdx, elementsSum, cache);
        return result;
    }

    if (nrRemainingElements == 1 && A[currentIdx] == elementsSum) {
        result = vector<vector<int>>(2, vector<int>());
        result[0].push_back(A[currentIdx]);
        for (int i = currentIdx + 1; i < A.size(); i++) {
            result[1].push_back(A[i]);
        }
        return result;
    }

    result = pick(elementsSum - A[currentIdx], nrRemainingElements - 1, currentIdx + 1, A, cache);
    if (!result.empty()) {
        result[0].insert(result[0].begin(), A[currentIdx]);
        return result;
    } else {
        result = pick(elementsSum, nrRemainingElements, currentIdx + 1, A, cache);
        if (!result.empty()) {
            result[1].insert(result[1].begin(), A[currentIdx]);
            return result;
        }
    }
    addToCache(nrRemainingElements, currentIdx, elementsSum, cache);

    return result;
}

vector<vector<int>> Solution::avgset(vector<int> &A) {
    sort(A.begin(), A.end());
    vector<vector<int>> result;
    int totalSum = 0;
    for (int i = 0; i < A.size(); i++) {
        totalSum += A[i];
    }
    vector<vector<vector<bool>>> cache;
    for (int sizeOfSet1 = 1; sizeOfSet1 <= A.size() / 2; sizeOfSet1++) {
        double sumOfSet1D = (double)sizeOfSet1 * totalSum / A.size();
        int sumOfSet1 = sumOfSet1D;
        //cout<<sumOfSet1D<<" "<<sumOfSet1<<endl;
        if (abs(sumOfSet1D - sumOfSet1) > 1e-8) continue;
        //cout<<sizeOfSet1<<" "<<sumOfSet1<<endl;
        result = pick(sumOfSet1, sizeOfSet1, 0, A, cache);
        if (!result.empty()) {
            return result;
        }
    }
    return result;
}

```

## Asked in
* Amazon
