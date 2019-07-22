# Simplify Directory Path

https://www.interviewbit.com/problems/simplify-directory-path



Given an absolute path for a file (Unix-style), simplify it.

Examples:

path = "/home/", => "/home"
path = "/a/./b/../../c/", => "/c"
Note that absolute path always begin with '/' ( root directory )
Path will not have whitespace characters.

More of a simulation problem. 
Break the string along '/' and process the substrings in order one by one. '..' indicates popping an entry unless there is nothing to pop.

## Solution

```cpp

/* editorial */

string Solution::simplifyPath(string s) {
    stack<string> st;
    int len = s.size();
    for (int i = 0; i < len; i++) {
        if (s[i] != '/') {
            string local = "";
            while (s[i] != '/') {
                local = local + s[i];
                i++;
                if (i >= len)
                    break;
            }
            if (local.compare(".") == 0)
                continue;
            if (local.compare("..") == 0) {
                if (!st.empty())
                    st.pop();
                continue;
            }
            st.push(local);
        }
    }
    string res;
    while (!st.empty()) {
        string local = st.top();
        reverse(local.begin(), local.end());
        res += local + "/";
        st.pop();
    }
    if (res.empty())
        res = "/";
    reverse(res.begin(), res.end());
    return res;
}


string Solution::simplifyPath(string A) {
   vector<string> collection;
    string name = "";
    
    A.push_back('/');   //For path ending with ..
    
    for (auto i = 0; i<A.length(); ++i)
    {
        if (A[i] == '/')
        {
            if (name.size() == 0) continue;
            else if (name == ".") {}
            else if (name == "..")
            {
                if (collection.size() > 0)
                    collection.pop_back();
            }
            else
                collection.emplace_back(name);
            
            name.clear();
        }
        else
            name.push_back(A[i]);
    }
    
    if (collection.empty())
        return "/";
        
    A.clear();
    for (auto j = 0; j<collection.size(); ++j)
        A.append("/" + collection[j]);
        
    return A;    
}

```