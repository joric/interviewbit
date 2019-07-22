# Redundant Braces

https://www.interviewbit.com/problems/redundant-braces

Write a program to validate if the input string has redundant braces?
Return 0/1
```
0 -> NO
1 -> YES
```

### Constraints

```
Input will be always a valid expression
Operators allowed are only + , * , - , /
```

### Example

```
((a + b)) has redundant braces so answer will be 1
(a + (a + b)) doesn't have have any redundant braces so answer will be 0
```

## Hint 1

Can you maintain a stack for removing a sub expression?

## Solution Approach

If we somehow pick out sub-expressions surrounded by ( and ), then if we are left with () as a part of the string, we know we have redundant braces.

Lets take an example:

```
(a+(a+b))
```

We keep pushing elements onto the stack till we encounter ')'. When we do encounter ')', we start popping elements till we find a matching '('. 

If the number of elements popped do not correspond to '()', we are fine and we can move forward. 

Otherwise, voila! Matching braces have been found.

### Some Extra Hints

```
Try to run your code on test cases like (a*(a))  and (a) ??
```

## Solution

```cpp

/* editorial */

class Solution {
  public:
	int braces(string str) {
		int N = str.size();
		stack<char> Stk;

		for (int i = 0; i < N; ++i) {
			if (str[i] == ')') {
				int cnt = 0;
				while (Stk.top() != '(') {
					Stk.pop();
					cnt++;
				}
				Stk.pop();
				if (cnt < 2)
					return 1;
			} else {
				Stk.push(str[i]);
			}
		}

		bool is = true;
		while (Stk.size()) {
			if (Stk.top() == '(' || Stk.top() == ')') {
				is = false;
				break;
			}
			Stk.pop();
		}

		if (!is)
			return 1;
		return 0;
	}
};

int Solution::braces(string A) {
	stack<char> s;
	auto size = A.length();
	auto i = 0;
	while (i < size) {
		char c = A[i];
		if (c == '(' || c == '+' || c == '*' || c == '-' || c == '/')
			s.push(c);
		else if (c == ')') {
			if (s.top() == '(')
				return 1;
			else {
				while (!s.empty() && s.top() != '(')
					s.pop();
				s.pop();
			}
		}
		++i;
	}
	return 0;
}
```