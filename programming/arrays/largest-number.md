# Largest Number

https://www.interviewbit.com/problems/largest-number/

Given a list of non negative integers, arrange them such that they form the largest number.

For example:

Given [3, 30, 34, 5, 9], the largest formed number is 9534330.

Note: The result may be very large, so you need to return a string instead of an integer.

## Hint 1

Hint : Sorting

Think around what kind of sorting would be needed. Obviously, we can't simply just sort the numbers or string.

Have you considered cases like 27, 271 or 12, 121 ?

## Solution approach

Sorting all numbers in descending order is the simplest solution that occurs to us. But this doesn't work.

For example, 548 is greater than 60, but in the output, 60 comes before 548. As a second example, 98 is greater than 9, but 9 comes before 98 in the output.

The solution is to use any comparison based sorting algorithm. Thus, instead of using the default comparison, write a comparison function myCompare() and use it to sort numbers.

Given two numbers X and Y, how should myCompare() decide which number to put first - we compare two numbers XY (Y appended at the end of X) and YX (X appended at the end of Y).

If XY is larger, then, in the output, X should come before Y, else Y should come before X.

For example, let X and Y be 542 and 60. To compare X and Y, we compare 54260 and 60542. Since 60542 is greater than 54260, we put Y first.


## Solution

```cpp
unsigned long long concatenate(int x, int y) {
    unsigned pow = 10;
    while(y >= pow)
        pow *= 10;
    return (unsigned long long)x * pow + y;
}

bool myfunction (int i, int j) { 
    return concatenate(i,j) > concatenate(j,i);
}

string Solution::largestNumber(const vector<int> &A) {
    vector<int> B = A; // get rid of const
    sort(B.begin(), B.end(), myfunction);
    if(B[0] == 0) return "0";
    string s;
    for (auto x:B)
        s += to_string(x);
    return s;
}


/*
bool myCompare(string X, string Y) { 
    string XY = X.append(Y); 
    string YX = Y.append(X);
    return XY.compare(YX) > 0 ? true: false;
} 

string Solution::largestNumber(const vector<int> &A) {
    vector<string> v;
    for (auto x:A)
        v.push_back(to_string(x));
    sort(v.begin(), v.end(), myCompare);
    string res;
    for (auto s:v)
        if (s!="0")
            res += s;
    if (res=="")
        res = "0";

    return res;
}
*/
```

## Asked in

* Amazon
* Goldman Sachs
* Microsoft


