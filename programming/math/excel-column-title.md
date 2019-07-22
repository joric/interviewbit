# Excel Column Title

https://www.interviewbit.com/problems/excel-column-title


## Solution

```cpp

string Solution::convertToTitle(int A) {
    string res;
    do {
        A--;
        res = char('A' + A % 26) + res;
        A /= 26;
    } while (A);
    return res;
}

/*

string Solution::convertToTitle(int A) {
    string res;
    do {
        A--;
        res += A % 26 + 'A';
        A /= 26;
    } while (A);
    reverse(res.begin(), res.end());
    return res;
}
*/
```