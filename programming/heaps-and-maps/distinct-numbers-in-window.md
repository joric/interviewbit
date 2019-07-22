# Distinct Numbers In Window

https://www.interviewbit.com/problems/distinct-numbers-in-window


If you have solution for window [i, i+k-1], can you quickly build solution for window [i+1, i+k], using some data structure?

What could be this data structure? A data structure which can store distinct elements(and their count?)?



If you have solution for window [i, i+k-1], can you quickly build solution for window [i+1, i+k]?

If we have a data structure where we can maintain count of all keys and number of distinct keys, then we just have to reduce count of key A[i] and increasing count of A[i+k]. If count of some key has been reduced to zero, we need to remove that key.

This structure is a hashmap. All operations that we have said a constant time in it.

## Solution

```cpp

// editorial

vector<int> Solution::dNums(vector<int> &A, int B) {
	assert(B <= A.size());
	int n = A.size();
	vector<int> ret;
	unordered_map<int, int> m;
	for (int i = 0; i < n; i++) {
		m[A[i]]++;
		if (i - B + 1 >= 0) {
			ret.push_back(m.size());
			m[A[i - B + 1]]--;
			if (m[A[i - B + 1]] == 0)
				m.erase(A[i - B + 1]);
		}
	}
	return ret;
}

vector<int> Solution::dNums(vector<int> &A, int B) {

	vector<int> ans;

	if (B > A.size()) {
		return ans;
	}

	map<int, int> m;
	int unique = 0;

	for (int i = 0; i < B; i++) {
		auto it = m.find(A[i]);
		if (it == m.end()) {
			m[A[i]] = 1;
			unique++;
		} else {
			it->second++;
		}
	}

	ans.push_back(unique);

	for (int i = B; i < A.size(); i++) {
		auto it = m.find(A[i - B]);
		if (it->second == 1) {
			m.erase(it);
			unique--;
		} else {
			it->second--;
		}
		auto temp = m.find(A[i]);
		if (temp != m.end()) {
			temp->second++;
		} else {
			m[A[i]] = 1;
			unique++;
		}
		ans.push_back(unique);
	}

	return ans;
}
```