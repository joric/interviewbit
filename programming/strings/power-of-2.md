# Power Of 2

https://www.interviewbit.com/problems/power-of-2



## Solution

```cpp

/* editorial */

string divide(string s, int &rem) {
	int n = s.size(), i;
	int carry = 0;
	string ret = "";
	for (i = 0; i < n; i++) {
		int k = carry * 10 + s[i] - '0';
		if (k % 2 != 0) {
			carry = 1;
		} else {
			carry = 0;
		}
		k /= 2;
		if (ret == "" && k == 0) {
			continue;
		}
		ret = ret + (char)(k + '0');
	}
	rem = carry;
	return ret;
}

int Solution::power(string A) {
	if (A == "1")
		return 0;
	while (A != "1") {
		//cout<<A<<endl;
		int r = 0;
		A = divide(A, r);
		if (r == 1)
			return 0;
	}
	return 1;
}

/* fastest*/

int Solution::power(string A) {
	long long k = 0;
	int i = 0;
	int len = A.size();
	while (i < len) {
		k = k * 10 + (A[i] - '0');
		i++;
	}
	if (k == 1)
		return 0;
	int p = k - 1;
	if ((k & p) == 0)
		return 1;
	else
		return 0;
}

/* lightweight */
int Solution::power(string A) {
	int i, j, k, l, m, n, num = 0;
	n = A.size();
	for (i = 0; i < n; i++) {
		num = num * 10 + (A[i] - '0');
		if (num > INT_MAX)
			return 0;
	}

	m = num;

	if (num == 1)
		return 0;
	m = (!(num & (num - 1)));
	return m;
}

/* another solution */

string multiplyByTwo(string A) {
	string ans = "";
	int sum = 0, carry = 0;
	for (int i = A.size() - 1; i >= 0; i--) {
		sum = (A[i] - '0') * 2 + carry;
		carry = sum / 10;
		sum = sum % 10;
		ans += (sum + '0');
	}
	if (carry > 0) {
		ans += (carry + '0');
	}
	for (int i = 0; i < ans.size() / 2; i++) {
		char c = ans[i];
		ans[i] = ans[ans.size() - i - 1];
		ans[ans.size() - i - 1] = c;
	}

	return ans;
}

bool compareString(string temp, string num) {
	if (temp.size() == num.size()) {
		return temp < num;
	} else if (temp.size() < num.size()) {
		return true;
	}
	return false;
}

int Solution::power(string A) {
	// Do not write main() function.
	// Do not read input, instead use the arguments to the function.
	// Do not print the output, instead return values as specified
	// Still have a doubt. Checkout www.interviewbit.com/pages/sample_codes/ for more details
	int i = 0;
	int length = A.size();
	while (A[i] == '0' && i < length) {
		i++;
	}
	if (i == length) {
		return 0;
	}

	string num = "";
	while (i != length) {
		num = num + A[i];
		i++;
	}

	string temp = "2";
	while (compareString(temp, num)) {
		temp = multiplyByTwo(temp);
	}

	if (temp == num) {
		return 1;
	}

	return 0;
}
```