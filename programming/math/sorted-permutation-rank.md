# Sorted permutation rank

https://www.interviewbit.com/problems/sorted-permutation-rank

Given a string, find the rank of the string amongst its permutations sorted lexicographically. 
Assume that no characters are repeated.

Example :

Input : 'acb'
Output : 2

The order permutations with letters 'a', 'c', and 'b' : 
```
abc
acb
bac
bca
cab
cba
```

The answer might not fit in an integer, so return your answer % 1000003

## Solution

```cpp
#define mod 1000003;

int fact (int n) {
	if (n == 0)
		return 1;
	else
		return (n * fact (n - 1)) % mod;
}

int Solution::findRank (string A) {
	string s = A;
	int ans = 0;
	int n = s.length ();
	for (int i = 0; i < n - 1; i++) {
		int c = 0;
		for (int j = i + 1; j < n; j++) {
			if (s[j] < s[i])
				c++;
		}
		ans += ((c * fact (n - i - 1))) % mod;
	}
	return (ans + 1) % mod;
}
```
