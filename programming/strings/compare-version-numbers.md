# Compare Version Numbers

https://www.interviewbit.com/problems/compare-version-numbers


Compare two version numbers version1 and version2.

If version1 > version2 return 1,
If version1 < version2 return -1,
otherwise return 0.
You may assume that the version strings are non-empty and contain only digits and the . character.
The . character does not represent a decimal point and is used to separate number sequences.
For instance, 2.5 is not "two and a half" or "half way to version three", it is the fifth second-level revision of the second first-level revision.

Here is an example of version numbers ordering:

0.1 < 1.1 < 1.2 < 1.13 < 1.13.4
## Solution

```cpp

int Solution::compareVersion(string A, string B) {
   string v1=A;
   string v2=B;
   unsigned long long int vnum1 = 0, vnum2 = 0; 
  
    //  loop untill both string are processed 
    for (int i=0,j=0; (i<v1.length() || j<v2.length()); ) 
    { 
        //  storing numeric part of version 1 in vnum1 
        while (i < v1.length() && v1[i] != '.') 
        { 
            vnum1 = vnum1 * 10 + (v1[i] - '0'); 
            i++; 
        } 
  
        //  storing numeric part of version 2 in vnum2 
        while (j < v2.length() && v2[j] != '.') 
        { 
            vnum2 = vnum2 * 10 + (v2[j] - '0'); 
            j++; 
        } 
  
        if (vnum1 > vnum2) 
            return 1; 
        if (vnum2 > vnum1) 
            return -1; 
  
        //  if equal, reset variables and go for next numeric 
        // part 
        vnum1 = vnum2 = 0; 
        i++; 
        j++; 
    } 
    return 0; 
}


#if 0

int Solution::compareVersion(string A, string B) {

    int i = 0, j = 0, n = A.size(), m = B.size();

    while (i < n && j < n) {
        string x = "";
        string y = "";

        while (i < n && A[i] == '0')
            i++;

        while (i < n && A[i] != '.') {
            x += A[i];
            i++;
        }

        while (j < m && B[j] == '0')
            j++;

        while (j < m && B[j] != '.') {
            y += B[j];
            j++;
        }

        if (x != y) {
            if (x.length() == y.length())
                return x.compare(y) > 0 ? 1 : -1;
            return x.length() > y.length() ? 1 : -1;
        }

        i++;
        j++;
    }

    while (i < n && A[i] == '0')
        i++;

    while (j < m && B[j] == '0')
        j++;

    if (i >= n && j >= m)
        return 0;

    return i > j ? 1 : -1;
}
#endif
```