# Elements which have at-least two greater elements

https://www.interviewbit.com/problems/elements-which-have-at-least-two-greater-elements/

Given an array of distinct integers A, find and return 
all elements in array which have at-least two greater elements than themselves.

Note: The results should have the order in which they are present in the original array.

### Input Format

The only argument given is the integer array A.

### Output Format

Return the elements which have at-least two greater elements than themselves.

### Constraints
```
3 <= length of the array <= 100000
-10^9 <= A[i] <= 10^9 
```

### For Example
```
Input 1:
    A = [1, 2, 3, 4, 5]
Output 1:
    [1, 2, 3]

Input 2:
    A = [5, 17, 100, 11]
Output 2:
    [5, 11]
```

## Solution
### Mine
```cpp
vector<int> Solution::solve(vector<int> &A) {
    A.erase(max_element(A.begin(), A.end()));
    A.erase(max_element(A.begin(), A.end()));
    return A;
}
```

### Another Mine
```cpp
vector<int> Solution::solve(vector<int> &A) {
    int a = INT_MIN, b = INT_MIN;
    for (int x:A) if (x>a) { b=a; a=x; } else if (x>b) b = x;
    for (int i=0, k=0; i<A.size(); i++) if (A[i]<b) A[k++] = A[i];
    A.resize(A.size()-2);
    return A;
}
```

