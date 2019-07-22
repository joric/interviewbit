# Shortest common superstring

Given a set of strings. Find the length of smallest string which
has all the strings in the set as substring

### Constraints:

```
1. 1 <= Number of strings <= 18
2. Length of any string in the set will not exceed 100.
```

### Example:

```
Input: ["abcd", "cdef", "fgh", "de"]
Output: 8 (Shortest string: "abcdefgh")
```

## Hint 1

If you were to use brute force, how would you solve the problem?
What if I say that Brute is O(N!) brutal?

Come on, there are not many approaches that click after saying O(N!), you are near.

## Solution Approach

### Brute force

Let's say we have only two strings say s1 and s2, the possible cases are:

1. They do not overlap [ans = len(s1) + len(s2) ]
2. They overlap partially [ans = len(s1)+len(s2)-len(max. overlapping part)]
3. They overlap completely [ans = max(len(s1), len(s2)]

What we can see here is we can easily combine two strings. In the brute force, we could take all the permutations of numbers [1 .. N], then combine the strings in that order.

e.g: strings = [s1, s2, s3], order = [2,3,1] 

Steps are: [s1, s2,s3] -> [s2+s3, s1] -> [s1+s2+s3].

(Here addition of strings is according to the method described above.

I would advice you to completely digest that this will give the optimal solution whatever the case may be. Considering all the permutations is optimal but time consuming.

### Dynamic programming

We have dynamic programming to our rescue in this case. You can see that there is a optimal substructure and overlapping subproblems in the brute force algorithm described above. Well if you can't already see, let me help you out.

#### Example

Input = [s1, s2, s3, s4]

Order 1 = [2,3,1,4] , Steps: [s2+s3, s1, s4] -> [s2+s3+s1, s4] -> [s1+s2+s3+s4]

Order 2 = [1,3,2,4] , Steps: [s1+s3, s2, s4] -> [s1+s2+s3, s4] -> [s1+s2+s3+s4].

Do you see here that Order1 and Order2 both calculated the optimal solution for set of strings [s1, s2, s3] 

(Intermediate string s1+s2+s3 is the optimal solution for this set)

Hurrah! Time to think Dynamically.

### Bitmasking in DP

Well, this kind of DP formulations require a specific technique called Bitmasking. It is not the conventional type and in this case T(N) = CCNN + CN*(2^N) (Still better than O(N!) right).

#### Formulation

dp[i][mask] = Optimal solution for set of strings corresponding to 1's in the mask where the last added string to our solution is i-th string.

#### Recurrence

dp[i][mask] = min(dp[x][mask ^ (1«i)] where {mask | (1«x) = 1} )

I recommend you reading about the Bitmask in DP if you still have the doubt.

Happy coding.

P.S.: For those feeling excited, you can try finding the string (not the length) once you complete this one.


## Solution

### Editorial
```cpp
#define INF 1 << 20
#define N 18

//Set the global variables
int targetMask;
int nonOverlapLen[N + 2][N + 2];
int dp[N][(1 << N) + 10];

// This can be done faster using KMP algorithm
//and some preprocessing
int combineString(vector<string> &words, int a, int b) {
    /*
    Let's say s1 = words[a] and s1 = words[b].
    
    If we append string s2 to the solution, then as string s1 is already
    added, we would need to add only the characters of string words[b] that are
    not overlapping with the solution.
    lenToADD = len(s2)-len(suffix of s1 which is also a prefix of s2);
    
    e.g.: s1 = "lara", s2 = "raghav"
          Now in the previous step, we added s1 to solution and let's say our solution
          at the time looks like "brilara" [made from strings "brila" and "lara"],
          if we add "raghav" to the solution, the only characters to be added are "ghav"
          and new solution = "brilaraghav"
    */

    //Only 1 string in the mask
    if (a == -1) {
        return words[b].size();
    }

    //Check if already calculated the ans
    if (nonOverlapLen[a][b] != -1) {
        return nonOverlapLen[a][b];
    }

    //Calculate the answer if not already calculated
    for (int i = 0; i < words[a].size(); ++i) {
        bool suffix = true;
        for (int j = 0; j < words[b].size() && i + j < words[a].size() && suffix; ++j) {
            if (words[a][i + j] != words[b][j]) suffix = false;
        }

        if (suffix) {
            int overlap = words[a].size() - i;
            nonOverlapLen[a][b] = words[b].size() - overlap;
            return nonOverlapLen[a][b];
        }
    }

    nonOverlapLen[a][b] = words[b].size();
    return nonOverlapLen[a][b];
}

//Main function to find the optimal solution
int findOptimal(vector<string> &words, int last, int mask) {
    //mask --> Set of strings that are considered for the solution.
    //last --> String that we added into the mask the last time
    //dp[i][mask] --> optimal answer for set of strings corresponding to mask(1's in mask) if the
    // last added string in mask is i-th string

    if (mask == targetMask) {
        //We considered all strings into the solution
        dp[last + 1][mask] = 0;
        return dp[last + 1][mask];
    }

    //Check if we solved this sub problem before
    if (dp[last + 1][mask] != -1) {
        return dp[last + 1][mask];
    }

    // Calculate the answer if not already calculated
    int ans = INF;

    for (int i = 0; i < words.size(); ++i) {
        //Check if i-th word is already considered or not
        if (!((mask >> i) & 1)) {
            ans = min(ans, findOptimal(words, i, mask | (1 << i)) + combineString(words, last, i));
        }
    }

    //Memoization step
    dp[last + 1][mask] = ans;

    return ans;
}

int Solution::solve(vector<string> &A) {
    int n = A.size();

    // Check the strings which are completely overlapping with some other string
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            if (i != j && A[i].find(A[j]) != string::npos) A[j] = "";
        }
    }

    //Make a new vector of only the strings which are not completely overlapping
    //with some other string

    vector<string> newA;
    for (int i = 0; i < n; i++) {
        if (A[i] != "") newA.push_back(A[i]);
    }

    //initialize the required arrays
    memset(nonOverlapLen, -1, sizeof(nonOverlapLen));
    memset(dp, -1, sizeof(dp));

    //Set the mask initially to all 0's
    int mask = 0;

    //Set the target mask
    targetMask = (1 << newA.size()) - 1;

    //Find the length of optimal solution
    int ans = findOptimal(newA, -1, mask);

    return ans;
}

```

### Fastest
```cpp

void mxstr(vector<string> &v) {
    int mx2, u, w, e, t, x, y, z, d, f, n = v.size();
    mx2 = 0;
    for (int j = 0; j < n - 1; j++) {
        for (int k = j + 1; k < n; k++) {
            string s1 = v[j];
            string s2 = v[k];
            if (s1.find(s2) != string::npos)
                v.erase(v.begin() + k);
            else if (s2.find(s1) != string::npos)
                v.erase(v.begin() + j);
            else {
                int mx, mx1, l = 0;
                mx = mx1 = 0;
                while (s2[l]) {
                    if (s2[l] == s1[0]) {
                        int r = l, p = 0, m = 0;
                        while (s2[r] && s2[r] == s1[p]) {
                            r++;
                            p++;
                            m++;
                        }
                        if (!s2[r] && m) {
                            mx = m;
                            t = p;
                            break;
                        }
                    }
                    l++;
                }
                l = 0;
                while (s1[l]) {
                    if (s1[l] == s2[0]) {
                        int r = l, p = 0, m = 0;
                        while (s1[r] && s1[r] == s2[p]) {
                            r++;
                            p++;
                            m++;
                        }
                        if (!s1[r] && m) {
                            mx1 = m;
                            x = p;
                            break;
                        }
                    }
                    l++;
                }
                if (mx >= mx1) {
                    f = mx;
                    y = k;
                    z = j;
                    d = t;
                } else {
                    f = mx1;
                    y = j;
                    z = k;
                    d = x;
                }
            }
            if (mx2 < f) {
                mx2 = f;
                u = y;
                w = z;
                e = d;
            }
        }
    }
    if (!mx2) {
        u = 0;
        w = 1;
        e = 0;
    }
    string q = v[w].substr(e);
    v[u] += q;
    v.erase(v.begin() + w);
}
int Solution::solve(vector<string> &A) {
    int n = A.size();
    while (A.size() != 1)
        mxstr(A);
    return A[0].length();
}

```

### Mine
```cpp
struct Element {
    int last;
    int len;
};

int overlap(const vector<string>& ss, vector<vector<int>>& overlap_seen,
    int first, int second) {
    if(first == -1) {
        return ss[second].size();
    }
    
    if(overlap_seen[first][second] != -1) {
        return overlap_seen[first][second];
    }

    string a = ss[first];
    string b = ss[second];

    if(a.find(b) != string::npos) {
        return 0;
    }

    int min_len = min(a.size(), b.size());
    int max_overlap = 0;
    for(int i = 1; i <= min_len; i++) {
        int pos = a.rfind(b.substr(0, i));
        if(pos + i == a.size()) {
            max_overlap = max(i, max_overlap);
        }
    }

    string leftover = b.substr(max_overlap, b.size() - max_overlap);
    
    overlap_seen[first][second] = b.size() - max_overlap;
    return overlap_seen[first][second];
}

// Time - O(N * 2^N), Space - O(2^N)
// where N = number of strings
int Solution::solve(vector<string> &A) {
    int n = A.size();
    for(int i = 0; i < n; i++) {
        for(int j = 0; j < n; j++) {
            if(i != j && A[i].find(A[j]) != string::npos) {
                swap(A.back(), A[j]);
                A.pop_back();
                n--;
                i--;
                j--;
            }
        }
    }
    
    vector<vector<int>> overlap_seen(n, vector<int>(n, -1));
    vector<Element> dp(pow(2, n), {-1, INT_MAX});
    dp[0] = {-1, 0};
    for(int mask = 0; mask < pow(2, n); mask++) {
        for(int j = 0; j < n; j++) {
            if((mask & (1 << j)) == 0) {
                int new_len = dp[mask].len + overlap(A, overlap_seen, dp[mask].last, j);
                int old_len = dp[mask | (1 << j)].len;

                if(new_len <= old_len) {
                    dp[mask | (1 << j)] = {j, new_len};
                }
            }
        }
    }
    
    return dp[pow(2, n) - 1].len;
}

```

## Asked in
* Google
