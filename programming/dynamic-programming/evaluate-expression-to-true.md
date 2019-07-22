# Evaluate Expression To True

Given expression with operands and operators (OR , AND , XOR) , in how many ways can you evaluate the expression to true, by grouping in different ways? Operands are only true and false.
### Input
```
string :  T|F&T^T
here '|' will represent or operator 
     '&' will represent and operator
     '^' will represent xor operator
     'T' will represent true operand
     'F' will false
```
### Output
```
different ways % MOD
where MOD = 1003
```
### Example:
```
string : T|F
only 1 way (T|F) => T
so output will be 1 % MOD = 1
```

## Hint 1
```
T|F
The operator here is 'or'. So, we need to find the number of ways sub-expression left of `|` operator,
or the sub-expression to the right of the operator, evaluates to true. 

In other words,
ways = (ways_T_left * ways_T_right) + (ways_F_left * ways_T_right) + (ways_T_left * ways_F_right)

because T | T = T
        T | F = T
        F | T = T

Can you extend the same analogy to other operators ? 
```

## Solution Approach
```
Assume Tr(i, j) tell us the number of ways to get True from sub-expresion i to j

Fa(i, j) tell us the number of ways to get False from subexpresion i to j. 


The recurrence relation will look like the following : 

some rules => 
or operator:
T|F = T
T|T = T
F|T = T
F|F = F

and operator:
T&F = F
T&T = T
F&T = F
F&F = F

x-or operator
T^T = F
T^F = T
F^T = T
F^F = F


for Tr(i, j) :
   Loop from i to j - 1 into variable k
     IF(k == AND) :
	Tr(i, j) = Tr(i, j) + (Tr(i, k) * Tr(k + 1, j))
     IF(k == OR) :
	Tr(i, j) = Tr(i, j) + (Tr(i, k) * Tr(k + 1, j)) + (Tr(i, k) * Fa(k + 1, j)) + (Fa(i, k) * Tr(k + 1, j))
     If(k == XOR) :
	Tr(i, j) = Tr(i, j) + (Tr(i, k) * Fa(k + 1, j)) + (Fa(i, k) * Tr(k + 1, j))

for Fa(i, j) :
 Loop from i to j - 1 into variable k
   IF(k == AND) :
     Fa(i, j) = Fa(i, j) + (Fa(i, k) * Fa(k + 1, j)) + (Fa(i, k) * Tr(k + 1, j)) + (Tr(i, k) * Fa(k + 1, j))
				
   IF(k == OR) :
	Fa(i, j) = Fa(i, j) + (Fa(i, k) * Fa(k + 1, j))
				
   If(k == XOR) :
	Fa(i, j) = Fa(i, j) + (Tr(i, k) * Tr(k + 1, j)) + (Fa(i, k) * Fa(k + 1, j))
```

## Solution

### Editorial
```cpp
#define pb push_back
#define mpa make_pair
#define ff first
#define ss second
#define MOD 1003

int Tr(int i, int j, string exp);

map<pair<int, int>, int> _True;
map<pair<int, int>, int> _False;

int Fa(int i, int j, string exp) {
    if (i > j)
        return 0;
    if (i == j)
        return exp[i] == 'F';

    if (_False.count(mpa(i, j)))
        return _False[mpa(i, j)];

    int ans = 0;
    for (int k = i; k < j; ++k) {
        if (exp[k] == '&')
            ans = ans + (Tr(i, k - 1, exp) * Fa(k + 1, j, exp)) + (Fa(i, k - 1, exp) * Tr(k + 1, j, exp)) + (Fa(i, k - 1, exp) * Fa(k + 1, j, exp));

        if (exp[k] == '|')
            ans = ans + (Fa(i, k - 1, exp) * Fa(k + 1, j, exp));

        if (exp[k] == '^')
            ans = ans + (Tr(i, k - 1, exp) * Tr(k + 1, j, exp)) + (Fa(i, k - 1, exp) * Fa(k + 1, j, exp));
        if (ans >= MOD)
            ans %= MOD;
    }

    return _False[mpa(i, j)] = ans;
}

int Tr(int i, int j, string exp) {
    if (i > j)
        return 0;
    if (i == j)
        return exp[i] == 'T';

    if (_True.count(mpa(i, j)))
        return _True[mpa(i, j)];

    int ans = 0;
    for (int k = i; k < j; ++k) {
        if (exp[k] == '&')
            ans = ans + (Tr(i, k - 1, exp) * Tr(k + 1, j, exp));

        if (exp[k] == '|')
            ans = ans + (Tr(i, k - 1, exp) * Tr(k + 1, j, exp)) + (Tr(i, k - 1, exp) * Fa(k + 1, j, exp)) + (Fa(i, k - 1, exp) * Tr(k + 1, j, exp)); // beacuase T OR T = > T  T OR F = > T  F OR T => T

        if (exp[k] == '^')
            ans = ans + (Tr(i, k - 1, exp) * Fa(k + 1, j, exp)) + (Fa(i, k - 1, exp) * Tr(k + 1, j, exp));
        if (ans >= MOD)
            ans %= MOD;
    }

    return _True[mpa(i, j)] = ans;
}

int Solution::cnttrue(string str) {
    _True.clear();
    _False.clear();

    int N = str.size();
    int ans = Tr(0, N - 1, str);
    return ans;
}

```

### Lightweight
```cpp
int Solution::cnttrue(string A) {
    int n, N = A.size(), i, k, j, l, MOD = 1003;
    n = 0;
    for (i = 0; i < N; i++) {
        if (A[i] == 'T' || A[i] == 'F')
            n++;
    }
    pair<int, int> dp[n][n];

    for (i = 0; i < n; i++) {
        if (A[2 * i] == 'T') {
            dp[i][i] = make_pair(1, 0);
        } else {
            dp[i][i] = make_pair(0, 1);
        }
    }

    for (l = 2; l <= n; l++) {
        for (i = 0; i + l <= n; i++) {
            pair<int, int> tmp;
            dp[i][i + l - 1] = make_pair(0, 0);
            for (j = 0; j < l - 1; j++) {
                char op = A[2 * (i + j) + 1];

                if (op == '|') {
                    tmp.first = dp[i][i + j].first * (dp[i + j + 1][i + l - 1].first + dp[i + j + 1][i + l - 1].second) + dp[i][i + j].second * dp[i + j + 1][i + l - 1].first;
                    tmp.second = dp[i][i + j].second * dp[i + j + 1][i + l - 1].second;
                } else if (op == '&') {
                    tmp.first = dp[i][i + j].first * dp[i + j + 1][i + l - 1].first;
                    tmp.second = dp[i][i + j].second * (dp[i + j + 1][i + l - 1].second + dp[i + j + 1][i + l - 1].first) + dp[i][i + j].first * dp[i + j + 1][i + l - 1].second;
                } else if (op == '^') {
                    tmp.first = dp[i][i + j].first * dp[i + j + 1][i + l - 1].second + dp[i][i + j].second * dp[i + j + 1][i + l - 1].first;
                    tmp.second = dp[i][i + j].first * dp[i + j + 1][i + l - 1].first + dp[i][i + j].second * dp[i + j + 1][i + l - 1].second;
                }
                dp[i][i + l - 1].first += tmp.first;
                dp[i][i + l - 1].first = dp[i][i + l - 1].first % MOD;
                dp[i][i + l - 1].second += tmp.second;
                dp[i][i + l - 1].second = dp[i][i + l - 1].second % MOD;
            }
        }
    }
    return dp[0][n - 1].first;
}


```

## Asked in
* Amazon