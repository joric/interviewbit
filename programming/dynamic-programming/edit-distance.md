# Edit Distance

https://www.interviewbit.com/problems/edit-distance/

Given two words A and B, find the minimum number of steps required to convert A to B. (each operation is counted as 1 step.)

You have the following 3 operations permitted on a word:

1. Insert a character
2. Delete a character
3. Replace a character

Example :

edit distance between

"Anshuman" and "Antihuman" is 2.

Operation 1: Replace s with t.

Operation 2: Insert i.

## Hint 1


Can you solve the problem for some prefix of first string and some prefix of second string and use it to compute the final answer?

Try to think of DP on prefixes of both strings.

## Solution Approach

This is a very standard Dynamic programming problem.

Lets look at the bruteforce approach for finding edit distance of S1 and S2.
We are trying to modify S1 to become S2.

We look at the first character of both the strings. 
If they match, we can look at the answer from remaining part of S1 and S2. 
If they don’t, we have 3 options. 
1) Insert S2’s first character and then solve the problem for remaining part of S2, and S1.
2) Delete S1’s first character and trying to match S1’s remaining string with S2.
3) Replace S1’s first character with S2’s first character in which case we solve the problem for remaining part of S1 and S2.

The code would probably look something like this :

```cpp
int editDistance(string &S1, int index1, string &S2, int index2) {
    // BASE CASES
    if (S1[index1] == S2[index2]) {
        return editDistance(S1, index1 + 1, S2, index2 + 1);
    } else {
        return min ( 1 + editDistance(S1, index1 + 1, S2, index2), // Delete S1 char
            1 + editDistance(S1, index1, S2, index2 + 1), // Insert S2 char
            1 + editDistance(S1, index1 + 1, S2, index2 + 1) // Replace S1 first char with S2 first char
        );
    }
}
```

The above approach is definitely exponential. 
However, lets look at the number of ways in which the function can be called. S1 and S2 remain constant. The only thing changing is index1 and index2 which can take len(S1) * len(S2) number of values. Can you use it to memoize ?

You can use the above observation to also come up with a non recursive solution.


## Solution

```cpp
int Solution::minDistance(string A, string B) {
    int row = A.size();
    int col = B.size();
    vector<vector<int> > temp(row+1, vector<int>(col+1));
    for(int i = 0; i < temp.size(); i++){
        for(int j = 0; j < temp[0].size(); j++){
            if(j == 0){
                temp[i][j] = i;
            }
            else if(i == 0){
                temp[i][j] = j;
            }
            else if(A[i-1] == B[j-1]){
                temp[i][j] = temp[i-1][j-1];
            }
            else{
                temp[i][j] = min(temp[i-1][j-1], temp[i-1][j]);
                temp[i][j] = min(temp[i][j-1], temp[i][j]);
                temp[i][j] = temp[i][j]+1;
            }
        }
    }
    
    return temp[row][col];
}
```

## Asked in

* Google
* LinkedIn
* Microsoft
* Amazon

