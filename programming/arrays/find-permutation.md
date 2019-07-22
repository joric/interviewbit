# Find Permutation

https://www.interviewbit.com/problems/find-permutation/

Given a positive integer n and a string s consisting only of letters D or I, you have to find any permutation of first n positive integer that satisfy the given input string.

D means the next number is smaller, while I means the next number is greater.

Notes

Length of given string s will always equal to n - 1
Your solution should run in linear time and space.
Example :

Input 1:

n = 3

s = ID

Return: [1, 3, 2]

## Hint 1

Try to keep an available range of element [l,r]. Initially, l=1 and r=n.

You always need to select smallest or largest element from available range. Try to figure out how and when.

## Solution Approach

When the input string contains only D or I we just need to return all positive number upto n either in descending or ascending orders respectively.
So if n = 3, s = II, return [1, 2, 3]

Now, starting with each character of the input string, we need to substitute an appropriate number(from 1 to n) corresponding to each character(I or D).

So, Suppose we started with a set corresponding to all the elements from that we need to make permutation(i.e all integer from 1 to n).

As I denotes the next number should be larger, we need to substitute smallest remaining number from our set corresponding to subsequent I as it automatically makes the next element to be larger.

Similar things will happens with character D, we need to substitute the largest remaining number from our set.

As the input string size is n - 1, we to append the last integer to our answer

Input:

n :  5

s = DIDD

Return: [5, 1, 4, 3, 2]


```cpp
// editorial

vector<int> Solution::findPerm(const string s, int n) {
    vector<int> ans(n);
    int a = 1, b = n;
    for (int i = 0; i < n && a < b; i++)
        ans[i] = s[i] == 'I' ? a++ : b--;
    ans[n - 1] = a;
    return ans;
}

// lightweight

vector<int> Solution::findPerm(const string A, int B) {

    vector<int> ans;

    int D = 0;

    for (int i = 0; i < A.length(); i++) {
        if (A[i] == 'D')
            D++;
    }
    // cout << D <<endl;
    int I = D + 1;
    // cout << I <<endl;

    ans.push_back(I);
    I++;

    for (int i = 0; i < A.length(); i++) {

        if (A[i] == 'D') {
            ans.push_back(D);
            // cout << D <<endl;
            D--;
        } else {
            ans.push_back(I);
            // cout << I <<endl;
            I++;
        }
    }

    return ans;
}

////////////////////////

vector<int> Solution::findPerm(const string A, int B) {
    int I = 0, i, c1 = 0, c2 = 0;
    vector<int> ans;
    for (i = 0; A[i] != '\0'; i++) {
        if (A[i] == 'I')
            I++;
    }
    c1 = B - I;
    c2 = c1 - 1;
    ans.push_back(c1);
    for (i = 0; A[i] != '\0'; i++) {
        if (A[i] == 'I') {
            c1++;
            ans.push_back(c1);
        } else {
            ans.push_back(c2);
            c2--;
        }
    }
    return ans;
}
```
