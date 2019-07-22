# Nearest Smaller Element

https://www.interviewbit.com/problems/nearest-smaller-element



Naive Solution O(N^2)

A naive solution would be to take a nested loop, and for every element keep iterating back till we find a smaller element.
This can be O(N^2) in worst case.

Better solution hint

The naive solution would look something like :

  for i = 0 to size(A):
    G[i] = -1
    for j = i - 1 to 0:
        if A[j] < A[i]:
            G[i] = A[j]
            break

Now look at A[i-1]. All elements with index smaller than i - 1 and greater than A[i-1]
are useless to us because they would never qualify for G[i], G[i+1], ...
Can you use the above fact to come up with a solution ?

Hint: Stack

Using the above fact, we know that we only need previous numbers in descending order.

The pseudocode would look something like :

1) Create a new empty stack st

2) Iterate over array `A`,
   where `i` goes from `0` to `size(A) - 1`.
    a) We are looking for value just smaller than `A[i]`. So keep popping from `st` till elements in `st.top() >= A[i]` or st becomes empty.
    b) If `st` is non empty, then the top element is our previous element. Else the previous element does not exist. 
    c) push `A[i]` onto st
## Solution

```cpp

/* editorial */

vector<int> Solution::prevSmaller(vector<int> &A) {
	vector<int> ans;
	ans.resize(A.size());

	stack<int> st;

	for (int i = 0; i < A.size(); i++) {
		while (!st.empty() && st.top() >= A[i])
			st.pop();
		if (st.empty()) {
			ans[i] = -1;
		} else {
			ans[i] = st.top();
		}
		st.push(A[i]);
	}
	return ans;
}

/* another solution */

vector<int> Solution::prevSmaller(vector<int> &A) {

	if (A.size() == 0) {
		return A;
	}

	vector<int> check(A.size(), 0);

	int smaller = A[0];

	for (int i = 0; i < A.size(); i++) {
		if (A[i] <= smaller) {
			check[i] = 1;
			smaller = A[i];
		}
	}

	vector<int> ans(A.size(), -1);

	int j = check.size() - 1;

	for (int i = check.size() - 1; i >= 0; i--) {
		if (check[i] == 0) {
			while (j >= i) {
				j--;
			}
			while (j >= 0) {
				if (A[i] > A[j]) {
					ans[i] = A[j];
					break;
				}
				j--;
			}
			if (j != -1) {
				j = i - 1;
			}
		}
	}

	return ans;
}
```