# Longest Consecutive Sequence

https://www.interviewbit.com/problems/longest-consecutive-sequence/

Given an unsorted array of integers, find the length of the longest consecutive elements sequence.

Example: 

Given [100, 4, 200, 1, 3, 2],

The longest consecutive elements sequence is [1, 2, 3, 4]. Return its length: 4.

Your algorithm should run in O(n) complexity.

## Solution

```cpp
// editorial

int Solution::longestConsecutive(const vector<int> &v) {
	unordered_set<int> h;
	for (int x : v)
		h.insert(x);

	int longestStreak = 0;

	for (int x : v) {
		int currentStreak = 1;
		int currentNum = x;

		if (h.count(currentNum - 1) == 0) {
			while (h.count(currentNum + 1) != 0) {
				currentNum++;
				currentStreak++;
			}
			longestStreak = max(longestStreak, currentStreak);
		}
	}

	return longestStreak;
}

// my
int Solution::longestConsecutive(const vector<int> &nums) {
	unordered_set<int> num_set;

	for (int num : nums)
		num_set.insert(num);

	int longestStreak = 0;

	for (int num : num_set) {
		if (!num_set.count(num - 1)) {
			int currentNum = num;
			int currentStreak = 1;

			while (num_set.count(currentNum + 1)) {
				currentNum += 1;
				currentStreak += 1;
			}

			longestStreak = max(longestStreak, currentStreak);
		}
	}

	return longestStreak;
}
```