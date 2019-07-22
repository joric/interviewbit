# 3 Sum

https://www.interviewbit.com/problems/3-sum


Given an array S of n integers, find three integers in S such that the sum is closest to a given number, target. 
Return the sum of the three integers.

Assume that there will only be one solution

Example: 
given array S = {-1 2 1 -4}, 
and target = 1.

The sum that is closest to the target is 2. (-1 + 2 + 1 = 2)

## Hint 1

The naive approach obviously is exloring all combinations of 3 integers using 3 loops.

Now, to look into improving, does it help if we sort the array?

## Solution Approach

As stated in the earlier hint, the naive approach is to have 3 loops of i,j,k over the array. We then just track S[i]+S[j]+S[k] for the case when (S[i]+S[j]+S[k]-target) is minimum. 
The code for the same looks something like the following :

```
  IF number of elements in S < 3
    THEN return -1; // Invalid case
  minDifference = abs(S[0] + S[1] + S[2] - target);
  bestTillNow = S[0] + S[1] + S[2];
  FOR i = 0 to size of S
    FOR j = i + 1 to size of S
      FOR k = j + 1 to size of S
        newDiff = abs(S[i] + S[j] + S[k] - target)
        IF newDiff < minDifference
          minDifference = newDiff
          bestTillNow = S[i] + S[j] + S[k]
        END IF
      END FOR
    END FOR
  END FOR
```
  bestTillNow is my answer. 

However, as stated earlier this approach is O(N^3). Lets see if we can do better.

Lets sort the array. 
When the array is sorted, try to fix the least integer by looping over it. 
Lets say the least integer in the solution is arr[i].

Now we need to find a pair of integers j and k, such that arr[j] + arr[k] is closest to (target - arr[i]). 
To do that, let us try the 2 pointer approach. 
If we fix the two pointers at the end ( that is, i+1 and end of array ), we look at the sum.

1. If the sum is smaller than the sum we need to get to, we increase the first pointer.
2. If the sum is bigger, we decrease the end pointer to reduce the sum.


## Solution

### Editorial

```cpp
int threeSumClosest (vector < int >&num, int target) {
	sort (num.begin (), num.end ());
	int bestSum = 1000000000, sum = 0;
	// Fix the smallest number in the three integers
	for (int i = 0; i < num.size () - 2; i++) {
		// Now num[i] is the smallest number in the three integers in the solution
		int ptr1 = i + 1, ptr2 = num.size () - 1;
		while (ptr1 < ptr2) {
			sum = num[i] + num[ptr1] + num[ptr2];
			if (abs (target - sum) < abs (target - bestSum)) {
				bestSum = sum;
			}
			if (sum > target) {
				ptr2--;
			}
			else {
				ptr1++;
			}
		}
	}
	return bestSum;
}
```
### Mine
```cpp
int Solution::threeSumClosest (vector < int >&nums, int target) {
	int min = nums[0] + nums[1] + nums[2];
	std::sort (nums.begin (), nums.end ());

	for (size_t a = 0; a < nums.size () - 2; a++) {
		int goal = target - nums[a];
		size_t left = a + 1;
		size_t right = nums.size () - 1;
		while (left < right) {
			int sum = nums[left] + nums[right];
			if (abs (sum - goal) < abs (min - target))
				min = sum - goal + target;
			if (sum < goal)
				while (left < right && nums[++left] == nums[left - 1]);
			else if (sum > goal)
				right--;
			else
				return target;
		}
	}
	return min;
}
```