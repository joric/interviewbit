# Window String

https://www.interviewbit.com/problems/window-string



Essentially you have a start and end pointer starting from the beginning
of the string. You keep moving the end pointer and including
more characters till you have all the characters of T included.
At this point, you start moving start pointer and popping out characters till
the point that you still have all the characters of T included.
Update your answer and keep repeating the process.

## Solution

```cpp

string Solution::minWindow(string S, string T) {
    unordered_map<char, int> m;
    for (auto c:T) m[c]++;
    int len = T.size(), i = 0, j = 0, maxlen = INT_MAX, start = 0;
    while (j < S.size()) {
        if (m[S[j++]]-- > 0)
            len--;
        while (len == 0) {
            if (j - i < maxlen) {
                maxlen = j - i;
                start = i;
            }
            if (m[S[i++]]++ == 0)
                len++;
        }
    }
    return maxlen == INT_MAX ? "" : S.substr(start, maxlen);
}

// misc

string Solution::minWindow(string S, string T) {
    auto sizeS = S.length();
    auto sizeT = T.length();

    if (sizeS < sizeT)
        return "";

    int needToFind[256] = { 0 };
    int hasFound[256] = { 0 };
    int count = 0;
    int minLength = INT_MAX;
    string res = "";

    for (auto i = 0; i < sizeT; ++i)
        needToFind[T[i]]++;

    for (int p = 0, q = 0; q < sizeS; ++q) {
        if (needToFind[S[q]] == 0)
            continue;

        hasFound[S[q]]++;

        if (hasFound[S[q]] <= needToFind[S[q]])
            ++count;

        if (count == sizeT) {
            while (hasFound[S[p]] == 0 || hasFound[S[p]] > needToFind[S[p]]) {
                if (hasFound[S[p]] > needToFind[S[p]])
                    hasFound[S[p]]--;
                ++p;
            }

            auto currLength = q - p + 1;
            if (currLength < minLength) {
                res.clear();
                res.append(S.begin() + p, S.begin() + q + 1);
                minLength = currLength;
            }
        }
    }

    return res;
}

// fail

string Solution::minWindow(string A, string B) {
    unordered_map<char, int> a;
    unordered_map<char, int> b;
    for (char c : A) a[c]++;
    for (char c : B) b[c]++;

    int i = 0, j = A.size() - 1;

    while (i <= j) {
        int i0 = i, j0 = j;

        char c = A[i];

        if (b[c]) {
            if (a[c] > 1) a[c]--, i++;
        } else
            i++;

        c = A[j];

        if (b[c]) {
            if (a[c] > 1) a[c]--, j--;
        } else
            j--;

        if (i == i0 && j == j0)
            break;
    }

    return A.substr(i, j - i + 1);
}
```