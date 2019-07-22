# Valid Ip Addresses

https://www.interviewbit.com/problems/valid-ip-addresses

Given a string containing only digits, restore it by returning all possible valid IP address combinations.

A valid IP address must be in the form of A.B.C.D, where A,B,C and D are numbers from 0-255. The numbers cannot be 0 prefixed unless they are 0.

### Example

Given `25525511135`, return `["255.255.11.135", "255.255.111.35"]`

(Make sure the returned strings are sorted in order)

## Solution

```cpp

bool isValid(string s) {
    if (s.size() > 1 && s[0] == '0')
        return false;
    if (stoi(s) <= 255 && stoi(s) >= 0)
        return true;
    else
        return false;
}

vector<string> Solution::restoreIpAddresses(string s) {
    vector<string> ans;
    if (s.size() > 12 || s.size() < 4)
        return ans;

    for (int i = 1; i < 4; i++) {
        string first = s.substr(0, i);
        if (!isValid(first))
            continue;
        for (int j = 1; i + j < s.size() && j < 4; j++) {
            string second = s.substr(i, j);
            if (!isValid(second))
                continue;
            for (int k = 1; i + j + k < s.size() && k < 4; k++) {
                string third = s.substr(i + j, k);
                string fourth = s.substr(i + j + k);
                if (isValid(third) && isValid(fourth)) {
                    string current = first + "." + second + "." + third + "." + fourth;
                    ans.push_back(current);
                }
            }
        }
    }
    return ans;
}
```