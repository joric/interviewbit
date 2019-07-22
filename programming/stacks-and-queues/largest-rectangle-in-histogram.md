# Largest Rectangle In Histogram

https://www.interviewbit.com/problems/largest-rectangle-in-histogram



We traverse all histograms from left to right, maintain a stack of histograms.
Every histogram is pushed to stack once.
A histogram is popped from stack when a histogram of smaller height is seen.
When a histogram is popped, we calculate the area with the popped histogram as smallest histogram.
How do we get left and right indexes of the popped histogram.
The current index tells us the 'right index' R and index of previous item in stack is the 'left index' L. 

## Solution

```cpp


// editorial

int Solution::largestRectangleArea(vector<int> &height) {
    int ret = 0;
    height.push_back(0);
    vector<int> index;

    for (int i = 0; i < height.size(); i++) {
        while (index.size() > 0 && height[index.back()] >= height[i]) {
            int h = height[index.back()];
            index.pop_back();

            int sidx = index.size() > 0 ? index.back() : -1;
            if (h * (i - sidx - 1) > ret)
                ret = h * (i - sidx - 1);
        }
        index.push_back(i);
    }

    return ret;
}

// array-based
int Solution::largestRectangleArea(vector<int> &height) {
    height.push_back(0);
    int len = height.size(), res = 0, cur = 1;
    int s[len + 1] = { 0 };
    s[0] = -1;
    for (int i = 1; i < len; i++) {
        while (cur && height[i] < height[s[cur]])
            res = max(res, height[s[cur]] * (i - s[--cur] - 1));
        s[++cur] = i;
    }
    return res;
}

int Solution::largestRectangleArea(vector<int> &A) {
    // Do not write main() function.
    // Do not read input, instead use the arguments to the function.
    // Do not print the output, instead return values as specified
    // Still have a doubt. Checkout www.interviewbit.com/pages/sample_codes/ for more details

    stack<int> st;

    int sol = 0;

    int i = 0, n = A.size();

    while (i < n) {
        if (st.empty() || A[st.top()] <= A[i]) {
            st.push(i);
            i++;
        } else {
            int top = st.top();
            st.pop();

            int area = 0;

            if (st.empty()) {
                area = A[top] * i;
            } else {
                area = A[top] * (i - st.top() - 1);
            }

            sol = max(sol, area);
        }
    }

    while (!st.empty()) {
        int top = st.top();
        st.pop();

        int area = 0;

        if (st.empty()) {
            area = A[top] * i;
        } else {
            area = A[top] * (i - st.top() - 1);
        }

        sol = max(sol, area);
    }

    return sol;
}
```