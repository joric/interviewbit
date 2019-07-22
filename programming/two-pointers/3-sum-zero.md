# 3 Sum Zero

https://www.interviewbit.com/problems/3-sum-zero


Given an array S of n integers, are there elements a, b, c in S such that a + b + c = 0? 
Find all unique triplets in the array which gives the sum of zero.

Elements in a triplet (a,b,c) must be in non-descending order. (ie, a ≤ b ≤ c)
The solution set must not contain duplicate triplets. For example, given array 
S = {-1 0 1 2 -1 -4}, A solution set is:
(-1, 0, 1)
(-1, -1, 2) 

  editorial
## Solution

```cpp


vector < vector < int >>threeSum (vector < int >&num) {
	sort (num.begin (), num.end ());
	int sum = 0;
	vector < vector < int >>ans;
	int sz = num.size ();
	// get the smallest number in the three integers
	for (int i = 0; i < sz - 2; i++) {
		// Lets make sure that we do not have duplicate triplets. 
		// We skip the duplicate elements as min integer in the triplet. 
		if (i > 0 && num[i] == num[i - 1]) {
			continue;
		}
		// Now num[i] is the smallest number in the three integers in the solution
		int ptr1 = i + 1, ptr2 = num.size () - 1;
		while (ptr1 < ptr2) {
			sum = num[i] + num[ptr1] + num[ptr2];

			if (sum == 0)
				ans.push_back(num[i], num[ptr1], num[ptr2]);

			if (sum > 0)
				ptr2--;
			else if (sum < 0)
				ptr1++;
			else { // sum == 0
				// In this case, we have a choice of increasing the first pointer, 
				// or decreasing the last pointer. Lets go wiht increasing the first 
				// pointer approach
				// Note that we cannot have duplicate triplets. So we need to skip all 
				// the occurrences of duplicates.
				ptr1++;
				while (ptr1 < ptr2 && num[ptr1] == num[ptr1 - 1]) {
					ptr1++;
				}
			}
		}
	}
	return ans;
}


vector < vector < int >>Solution::threeSum (vector < int >&a) {
	int n = a.size ();
	sort (a.begin (), a.end ());
	vector < vector < int >>results;

	for (int i = 0; i < n - 2; i++) {

		int j = i + 1;
		int k = n - 1;

		if (i > 0 && a[i] == a[i - 1])	// pass duplicates for i
			continue;

		while (j < k) {

			if (j > i + 1 && a[j] == a[j - 1]) {	// pass duplicates for j
				j++;
				continue;
			}

			if (k < n - 1 && a[k] == a[k + 1]) {	// pass duplicates for k
				k--;
				continue;
			}

			if (a[i] + a[j] + a[k] == 0) {
				results.push_back ( {
								   a[i], a[j], a[k]}
				);
				j++;
				k--;

			}
			else if (a[i] + a[j] + a[k] > 0) {
				k--;
			}
			else {
				j++;
			}
		}
	}

	return results;
}
```