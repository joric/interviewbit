# Zigzag String

https://www.interviewbit.com/problems/zigzag-string


More of a simulation problem. Think how you can simulate this?

Just look at simply simulating what is being told in the problem.

Follow the simple steps:

You need to maintain numRows number of strings S[numRows].
And then populating string S in each row in zigzag fashion.
Finally concatenate S[0] .. S[numRows-1] to get the answer.
## Solution

```cpp

string Solution::convert(string s, int rows) {
    string ans;
    if(rows<=1)
        return s;
    int i=0,j=0, n = s.length(), l = rows-1;
    while(i<rows){
        j=i;
        while(j<n && ans.length()<n){
            ans.append(1,s[j]);
            j+= (l -(j%l))*2;
        }
        i++;
    }
    return ans;
}


string Solution::convert(string A, int B) {
    int i = 0;
    int count = 0;
    int flag = 0;
    int num = B - 1;

    string ans = "";

    if (A.size() == 0 || A.size() == 1 || B == 1) {
        return A;
    }

    while (count < B) {
        i = count;
        flag = 0;
        while (i < A.size()) {
            if ((count == 0) || (count == B - 1)) {
                ans = ans + A[i];
                i = i + 2 * num;
            } else {
                if (flag == 0) {
                    ans = ans + A[i];
                    i = i + 2 * (B - count - 1);
                    flag = 1;
                } else {
                    ans = ans + A[i];
                    i = i + 2 * count;
                    flag = 0;
                }
            }
        }
        count++;
    }

    return ans;
}
```