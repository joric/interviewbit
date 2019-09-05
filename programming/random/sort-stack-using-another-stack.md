# Sort stack using another stack

https://www.interviewbit.com/problems/sort-stack-using-another-stack/

Given a stack of integers A, sort it using another stack.

Return the array of integers after sorting the stack using another stack.

### Input Format

The only argument given is the integer array A.

### Output Format

Return the array of integers after sorting the stack using another stack.

### Constraints
```
1 <= length of the array <= 5000
0 <= A[i] <= 10^9 
```

### For Example
```
Input 1:
    A = [5, 4, 3, 2, 1]
Output 1:
    [1, 2, 3, 4, 5]

Input 2:
    A = [5, 17, 100, 11]
Output 2:
    [5, 11, 17, 100]
```

## Solution Approach

Create a temporary stack say B.

While input stack is not empty:

1. pop an element from input stack calls it x.
2. while the temporary stack is not empty and top of the temporary stack is greater than x pop from the temporary stack and push it into input stack.
3. push x in the temporary stack.

The sorted numbers are in the temporary stack.

Worst case time complexity O(n^2).

## Solution
### Editorial
```cpp
vector<int> Solution::solve(vector<int> &A) {
    if(A.size()<=1) return A;
    stack<int>s1,helper;
    for(int i=0;i<A.size();i++){
        s1.push(A[i]);
    }
    while(!s1.empty()){
        int temp = s1.top();
        s1.pop();
        while(!helper.empty()&&helper.top()>temp){
            s1.push(helper.top());
            helper.pop();
        }
        helper.push(temp);
    }
    while(!helper.empty()){
        s1.push(helper.top());
        helper.pop();
    }
    vector<int>arr;
    while(!s1.empty()){
        arr.push_back(s1.top());
        s1.pop();
    }
    return arr;
}
```

### Fastest
```cpp
vector<int> Solution::solve(vector<int> &A) {
    sort(A.begin(),A.end());
    return A;
}
```
### Lightweight
```cpp
vector<int> Solution::solve(vector<int> &A) {
    vector<int> res;
    
    while(!A.empty()) {
        int x = A.back();
        A.pop_back();
        while(!res.empty()&&res.back()>x)A.push_back(res.back()),res.pop_back();
        res.push_back(x);
        while(!A.empty()&&A.back()>=res.back())res.push_back(A.back()),A.pop_back();
    }
    
    return res;
}
```

### Mine
```cpp
vector<int> Solution::solve(vector<int> &A) {
    stack<int> input;
    for (int x:A) input.push(x);
    
    stack<int> output; 
    while (!input.empty()) { 
        int tmp = input.top(); 
        input.pop(); 
        while (!output.empty() && output.top() < tmp) { 
            input.push(output.top()); 
            output.pop(); 
        } 
        output.push(tmp); 
    }
    
    vector<int> res;
    while (!output.empty()) {
        res.push_back(output.top());
        output.pop();
    }
    return res;
}
```
