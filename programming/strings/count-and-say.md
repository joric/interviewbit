# Count And Say

https://www.interviewbit.com/problems/count-and-say/

The count-and-say sequence is the sequence of integers beginning as follows:
```
1, 11, 21, 1211, 111221, ...
1 is read off as one 1 or 11.
11 is read off as two 1s or 21.
```

21 is read off as one 2, then one 1 or 1211.

Given an integer n, generate the nth sequence.

Note: The sequence of integers will be represented as a string.

Example:

if n = 2, the sequence is 11.

## Hint 1

You need to figure out how the new sequence is constructed from old sequence by counting contiguous same numbers.

## Solution Approach

The best way is to simulate.

Try constructing the sequence starting from 1.

For constructing the current sequence, you need to look at the previous sequence and count the size of the contiguous sequences.

## Solution

### Editorial
```cpp
string Solution::countAndSay(int n) {
    if (n == 0) return "";
    if (n == 1) return "1";

    string prev = "1";
    string current = prev;

    for (int seqNum = 2; seqNum <= n; seqNum++) { // run from starting to generate second string

        current.clear();
        //cheack all digits in the string
        for (int j = 0; j < prev.length(); j++) {

            int count = 1; // we have at least 1 occourence of each digit

            // get the number of times same digit occurred (be carefull with the end of the string)
            while ((j + 1 < prev.length()) && (prev[j] == prev[j + 1])) {
                count++;
                j++; // we need to keep increasing the index inside of the string
            }

            // add to new string "count" + "digit itself"
            current.append(to_string(count) + prev[j]);
        }

        // save temporary result
        prev = current;
    }

    return current;
}

```

### Mine
```cpp
string Solution::countAndSay(int A) {
    string result;
    if (!A)
        return result;
    string str = "1";
    int cnt = 1;
    for (int i = 1; i < A; ++i) {
        int len = str.length();
        for (int j = 0; j < len; ++j) {
            if (j + 1 != len && str[j] == str[j + 1]) {
                ++cnt;
            } else {
                result += to_string(cnt) + str[j];
                cnt = 1;
            }
        }
        str = result;
        result.clear();
    }
    return str;
}

```

## Asked in
* Facebook
