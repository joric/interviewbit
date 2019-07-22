# Count Element Occurence

https://www.interviewbit.com/problems/count-element-occurence

 easy std solutions */
int Solution::findCount(const vector<int> &A, int B) {
    auto lower = lower_bound(A.begin(), A.end(), B);
    auto upper = upper_bound(A.begin(), A.end(), B);
    return distance(lower, upper); // or just upper - lower
}

int Solution::findCount(const vector<int> &A, int B) {
    auto range = equal_range(A.begin(), A.end(), B);
    return distance(range.first, range.second); // or range.second-range.first
}

int Solution::findCount(const vector<int> &A, int B) {
    typedef vector<int>::const_iterator it;
    pair<it, it> range = equal_range(A.begin(), A.end(), B);
    return range.second - range.first;
}


/* editorial */

int Solution::findCount (const vector < int >&A, int target) {
    int n = A.size ();
    int i = 0, j = n - 1;
    int start = -1, end = -1;

    // FIND FIRST
    while (i < j) {
        int mid = (i + j) / 2;
        if (A[mid] < target)
            i = mid + 1;
        else
            j = mid;
    }
    if (A[i] != target)
        return 0;                // the element does not exist in the array.

    start = i;

    // FINDLAST
    j = n - 1;                    // We don't have to set i to 0 the second time.
    while (i < j) {
        int mid = (i + j) / 2 + 1;    // Make mid biased to the right
        if (A[mid] > target)
            j = mid - 1;
        else
            i = mid;            // So that this won't make the search range stuck.
    }
    end = j;
    return (end - start + 1);
}


/* something (also works) */


int BinarySearch (const vector < int >&A, int N, int x, int searchFirst) {
	// search space is A[low..high]
	int low = 0, high = N - 1;

	// initialize the result by -1
	int result = -1;

	// iterate till search space contains at-least one element
	while (low <= high) {
		// find the mid value in the search space and
		// compares it with target value
		int mid = (low + high) / 2;

		// if target is found, update the result
		if (x == A[mid]) {
			result = mid;

			// go on searching towards left (lower indices)
			if (searchFirst)
				high = mid - 1;

			// go on searching towards right (higher indices)
			else
				low = mid + 1;
		}

		// if target is less than the mid element, discard right half
		else if (x < A[mid])
			high = mid - 1;

		// if target is more than the mid element, discard left half
		else
			low = mid + 1;
	}

	// return the found index or -1 if the element is not found
	return result;
}

int Solution::findCount (const vector < int >&A, int B) {
	// pass value 1 for first occurrence
	int n = A.size (), target = B;

	int first = BinarySearch (A, n, target, 1);

	// pass value 0 for last occurrence
	int last = BinarySearch (A, n, target, 0);

	int count = last - first + 1;

	if (first != -1)
		return count;

	return 0;
}
```