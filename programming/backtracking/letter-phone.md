# Letter Phone

https://www.interviewbit.com/problems/letter-phone


## Hint 1

Think about possibilites at any place and move on.

## Hint 2

For every integer, you have 1/3/4 options. Try appending every letter in the option to the string and move forward.
## Solution

```cpp

/* fastest */

string mapping[] = { "0", "1", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz" };

void makeCombinations(const string &A, const int pos, const string &current, vector<string> &result) {
	if (pos >= A.size())
		return;
	const string &letters = mapping[A[pos] - '0'];
	for (const char ch : letters) {
		string next = current;
		next.push_back(ch);
		if (next.size() == A.size()) {
			result.push_back(next);
		} else {
			makeCombinations(A, pos + 1, next, result);
		}
	}
}

vector<string> Solution::letterCombinations(string A) {
	vector<string> result;
	makeCombinations(A, 0, "", result);
	return result;
}

/* my */

unordered_map<char, string> h = { { '0', "0" }, { '1', "1" }, { '2', "abc" }, { '3', "def" }, { '4', "ghi" }, { '5', "jkl" }, { '6', "mno" }, { '7', "pqrs" }, { '8', "tuv" }, { '9', "wxyz" } };

void helper(vector<string> &res, string comb, string &digits, int pos) {
	if (pos == digits.size()) {
		res.push_back(comb);
		return;
	}
	for (char ch : h[digits[pos]])
		helper(res, comb + ch, digits, pos + 1);
}

vector<string> Solution::letterCombinations(string A) {
	if (A.empty())
		return {};
	vector<string> res;
	string comb;
	helper(res, comb, A, 0);
	return res;
}
```