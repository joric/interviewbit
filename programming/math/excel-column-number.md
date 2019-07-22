# Excel Column Number

https://www.interviewbit.com/problems/excel-column-number


## Solution

```cpp
int Solution::titleToNumber(string A) {

    int x = 0, p = 1;
    for (int i=A.length()-1; i>=0; i--) {
        x += int(A[i]-'A'+1) * p;
        p *= 26;
    }

    return x;
}

```