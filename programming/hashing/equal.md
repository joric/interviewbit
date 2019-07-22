# Equal

https://www.interviewbit.com/problems/equal


## Solution

```cpp


// editorial

vector<int> Solution::equal(vector<int> &vec) {
    int N = vec.size();
    // With every sum, we store the lexicographically first occuring pair of integers.
    map<int, pair<int, int>> h;
    vector<int> Ans;

    for (int i = 0; i < N; ++i) {
        for (int j = i + 1; j < N; ++j) {

            int Sum = vec[i] + vec[j];

            if (h.find(Sum) == h.end()) {
                h[Sum] = make_pair(i, j);
                continue;
            }

            pair<int, int> p1 = h[Sum];
            if (p1.first != i && p1.first != j && p1.second != i && p1.second != j) {
                vector<int> ans;
                ans.push_back(p1.first);
                ans.push_back(p1.second);
                ans.push_back(i);
                ans.push_back(j);

                if (Ans.size() == 0)
                    Ans = ans;
                else {
                    // compare and assign Ans
                    bool shouldReplace = false;
                    for (int i1 = 0; i1 < Ans.size(); i1++) {
                        if (Ans[i1] < ans[i1])
                            break;
                        if (Ans[i1] > ans[i1]) {
                            shouldReplace = true;
                            break;
                        }
                    }
                    if (shouldReplace)
                        Ans = ans;
                }
            }
        }
    }

    return Ans;
}


// fastest

vector<int> Solution::equal(vector<int> &A) {
    int n = A.size();
    for (int i = 0; i < n - 3; i++)
        for (int j = i + 1; j < n - 1; j++) {
            unordered_map<int, int> m;
            vector<int> v;
            for (int k = i + 1; k < n; k++) {
                if (k == j)
                    continue;
                int comp = A[i] + A[j] - A[k];
                if (m.find(comp) != m.end()) {
                    if (v.empty() || m[comp] < v[0])
                        v = { m[comp], k };
                }
                if (m.find(A[k]) == m.end())
                    m[A[k]] = k;
            }
            if (!v.empty())
                return { i, j, v[0], v[1] };
        }
    return {};
}

// lightweight

vector<int> Solution::equal(vector<int> &A) {
    vector<int> out;
    int size = A.size();
    int i, j, k, l;

    for (i = 0; i < n; i++)
        for (j = 0; j < n; j++)
            for (k = 0; k < n; k++)
                for (l = 0; l < n; l++)
                    if (i < j && k < l && i < k && j != k && j != l && (A[i] + A[j] == A[k] + A[l]))
                        return out(i, j, k, l);
    return out;
}

```