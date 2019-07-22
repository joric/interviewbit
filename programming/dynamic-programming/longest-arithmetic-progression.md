# Longest Arithmetic Progression

https://www.interviewbit.com/problems/longest-arithmetic-progression/


Find longest Arithmetic Progression in an integer array and return its length.
More formally, find longest sequence of indeces,

0 < i1 < i2 < ...  < ik < ArraySize(0-indexed) such that sequence
A[i1], A[i2], ... , A[ik] is an Arithmetic Progression.

Arithmetic Progression is a sequence in which all the differences
between consecutive pairs are the same, i.e sequence

B[0], B[1], B[2], ... , B[m - 1] of length m is an Arithmetic Progression if and only if

B[1] - B[0] == B[2] - B[1] == B[3] - B[2] == ...  == B[m - 1] - B[m - 2].

### Examples

```
1) 1, 2, 3 (All differences are equal to 1)
2) 7, 7, 7 (All differences are equal to 0)
3) 8, 5, 2 (Yes, difference can be negative too)
```

### Samples

```
1) Input: 3, 6, 9, 12
Output: 4
2) Input: 9, 4, 7, 2, 10
Output: 3(If we choose elements in positions 1, 2 and 4(0-indexed))
```

## Hint 1

What if we have first two elements of some Arithmetic Progression? Think about bruteforce.

## Hint 2

Bruteforce solution. Let n be the length of input array. Iterate all over pairs 0 <= i < j < n and build Arithmetic Progression that has first two elements A[i], A[j].
```
for i : [0..n - 1]
	for j : [i + 1..n - 1]
		cur = 2
		lst = A[j]
		dif = A[j] - A[i]
		for k : [j + 1..n - 1]
			if (A[k] == lst + dif)
				cur++
				lst = A[k]
		ans = max(ans, cur)
```
It’s O(n ^ 3) solution. Think about Dynamic Programming.

## Solution Approach

Let dp[i][j] be the length of Longest Arithmetic progression that ends in positions i and j, i.e. last element is A[j] and element before last is A[i]. How can we calculate a value for fixed i and j? We know two last elements. So we know which number should be before position i. It’s number X such that A[i] - X == A[j] - A[i] -> X == 2 * A[i] - A[j]. I.e we can iterate all over 0 <= k < i and if A[k] == X then update dp[i][j] by the value of dp[k][i] + 1(it’s easy to understand we only need to find rightmost such position).
```
for i : [0..n - 1]
	for j : [i + 1..n - 1]
		dp[i][j] = 2
		dif = A[j] - A[i]
		/*
		a[i] - x == a[j] - a[i]
		x == 2 * a[i] - a[j]
		*/
		need = 2 * A[i] - A[j]
		pos = -1
		for k : [0..i - 1]
			if (a[k] == need) 
				pos = k
		if (pos <> -1) 
			dp[i][j] = max(dp[i][j], dp[pos][i] + 1)
```

it's still **O(n ^ 3)** solution, because for fixed pair(i, j)
we need **O(n)** time to find pos. Think how can we find this position quickier.

## Solution

### Tutorial
```cpp
//For searching some numbers in the prefix lets use some data structure.
// Lets store associative array [number, position].

int Solution::solve(const vector<int> &A) {
    int n = A.size();
    if (n < 3) return n;
    vector<vector<int>> dp(n, vector<int>(n, -1));
    map<int, int> pos;
    for (int i = 0; i < n; i++) {
        for (int j = i + 1; j < n; j++) {
            dp[i][j] = 2;
            int dif = A[j] - A[i];
            int need = 2 * A[i] - A[j];
            if (pos.count(need) == 0) continue;
            dp[i][j] = max(dp[i][j], dp[pos[need]][i] + 1);
        }
        pos[A[i]] = i;
    }
    int ans = 2;
    for (int i = 0; i < n; i++) {
        for (int j = i + 1; j < n; j++) {
            ans = max(ans, dp[i][j]);
        }
    }
    return ans;
}

//it’s O(n ^ 2 * log(n)) solution.
//Also we can use unordered map(hash map) here,
// but its running time is undetermined
//(it's **O(1)** in average, but constant might be too big).
```

### Lightweight
```cpp
int Solution::solve(const vector<int> &A) {
    int s = A.size(), ans = 2;
    if (s <= 1)
        return s;
    for (int i = 0; i < s; i++) {
        int first = A[i];
        for (int j = i + 1; j < s; j++) {
            long long last = A[j];
            long long cd = A[j] - A[i];
            int len = 2;
            for (int k = j + 1; k < s; k++) {
                if (A[k] - last == cd) {
                    last = A[k];
                    len++;
                }
            }
            ans = max(ans, len);
        }
    }
    return ans;
}

```

### Mine
```cpp
int Solution::solve(const vector<int> &v) {
    unordered_map<int, int> m;
    int i, n = v.size(), j;
    if (n == 1)
        return 1;
    if (n == 0)
        return 0;
    vector<int> dp(n, 2);
    for (i = 1; i < v.size(); i++) {
        for (j = 0; j < i; j++) {
            int x = v[i] - v[j];
            if (m.find(x) != m.end()) {
                if (dp[i] < dp[j] + 1)
                    dp[i] = dp[j] + 1;
            } else
                m[x] = 1;
        }
    }
    return *max_element(dp.begin(), dp.end());
}

```
