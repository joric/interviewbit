# Cows2

https://www.interviewbit.com/problems/cows2


## Solution

```cpp
int F (vector < int >&A, int x, int C) {
	//We can always place the first cow in the leftmost stall
	int N = pos.size ();
	int cowsplaced = 1, lastpos = pos[0];
	for (int i = 1; i < N; i++) {
		if (A[i] - lastpos >= x) {
			if (++cowsplaced == C)
				return 1;
			lastpos = A[i];
		}
	}
	return 0;
}

int Solution::solve (vector < int >&A, int B) {
	int N = A.size ();
	sort (A.begin (), A.end ());
	int start = 0, end = A[N - 1] - A[0] + 1, mid;
	while (end - start > 1) {
		mid = (end + start) >> 1;
		(F (A, mid, B) ? start : end) = mid;
	}
	return start;
}
```