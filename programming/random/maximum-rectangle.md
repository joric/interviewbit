# Maximum Rectangle

https://www.interviewbit.com/problems/maximum-rectangle

Given a 2D binary matrix of integers A containing 0’s and 1’s of size N x M.

Find the largest rectangle containing only 1’s and return its area.

Note: Rows are numbered from top to bottom and columns are numbered from left to right.

### Input Format

The only argument given is the integer matrix A.

### Output Format

Return the area of the largest rectangle containing only 1's.

### Constraints
```
1 <= N, M <= 1000
0 <= A[i] <= 1
```
### For Example
```
Input 1:
    A = [   [0, 0, 1]
            [0, 1, 1]
            [1, 1, 1]   ]
Output 1:
    4

Input 2:
    A = [   [0, 1, 0, 1]
            [1, 0, 1, 0]    ]
Output 2:
    1
```

## Solution
### Editorial
```cpp
int largest_rectangle(vector<int>hist)
    {
        //print_hist(hist);
        int top, max_area =-1, area,i;
        stack<int>st;//store index
        for(i = 0; i < hist.size(); i++)
        {
            if(st.empty() || hist[st.top()] <= hist[i])
                st.push(i);
            else
            {//upto stack becomes empty or current value >= stack top value
                while(!st.empty() && hist[i] < hist[st.top()])
                {
                    top = st.top();
                    st.pop();
                    if(st.empty())
                        area = hist[top] * i;//when stack empty
                    else
                        area = hist[top] * (i - st.top() -1);
                    max_area = max_area > area ? max_area : area ;
                }
                st.push(i); //current index
            }

        }
        //same as above code
        while(!st.empty())
        {
            top = st.top();
            st.pop();
            if(st.empty())
                area = hist[top] * i;
            else
                area = hist[top] * (i - st.top() -1);
            max_area = max_area > area ? max_area : area;
        }
        //cout<<max_area<<endl;
        return max_area;
    }

int maximalRectangle(vector<vector<int>>& matrix) {
        if(matrix.size() == 0)
            return 0;
        int row = matrix.size(),col=matrix[0].size(),flag=0;
        //cout<<row<<" "<<col<<endl;
        if(row > col)
        {
            swap(row,col);
            flag=1;
        }
        //cout<<row<<" "<<col<<endl;
        int i,j,max_area=0;
        vector<int>hist(col,0);
        for(i = 0; i < row; i++)
        {
            for(j = 0; j < col; j++)
            {
                char c;
                if(flag)
                    c= matrix[j][i]; //if row > col
                else
                    c = matrix[i][j];
                //cout<<c<<' ';
                hist[j] = (c ==0) ? 0 : hist[j]+1;
            }
            //cout<<endl;
            max_area = max(max_area, largest_rectangle(hist));
//            print_hist(hist);
        }
        return max_area;
    }

int Solution::solve(vector<vector<int> > &A) {
    return maximalRectangle(A);
}
```

### Fastest
```cpp
int histogram(vector<int> &A) {
    int i,j,n=A.size();
    i=0;
    int area,maxarea=INT_MIN,tp;
    stack<int> st;
    while(i<n)
    {
            while(!st.empty() && A[i]<A[st.top()])
            {
                tp=st.top();
                st.pop();
                if(!st.empty())
                area=A[tp]*(i-st.top()-1);
                else
                area=A[tp]*i;
                
                maxarea=max(area,maxarea);
            }
            st.push(i);
             i++;
    }
    while(!st.empty())
        {
            tp=st.top();
            st.pop();
            if(!st.empty())
            area=A[tp]*(i-st.top()-1);
            else
            area=A[tp]*i;
            
            maxarea=max(area,maxarea);
        }
    return maxarea;
}
int Solution::solve(vector<vector<int> > &A) {
    int i,j,n=A.size(),m=A[0].size();
    int mxarea=INT_MIN;
    vector<int> v(A[0]);
    mxarea=max(mxarea,histogram(v));
    for(i=1;i<n;i++)
    {
        for(j=0;j<m;j++)
        {
            if(A[i][j]==1)
            v[j]++;
            else
            v[j]=0;
        }
        mxarea=max(mxarea,histogram(v));
    }
    return mxarea;
}
```

### Lightweight
```cpp
int f(const vector<int>& hist) {
    int res = 0, n = hist.size();
    stack<int> st;
    st.push(-1);
    
    for (int i = 0; i <= n; ++i) {
        while(!st.empty()) {
            int j = st.top();
            if(j<0||i!=n&&hist[j]<hist[i])break;
            st.pop();
            if(!st.empty()) {
                int r = (i - st.top() - 1)*hist[j];
                if(r>res)res=r;
            }
        }
        st.push(i);
    }
    
    return res;
}


int Solution::solve(vector<vector<int> > &A) {
    int n = A.size(), m = A[0].size(), res = 0;
    vector<int> hist(m, 0);
    for (int i = 0; i < n; ++i) {
        for(int j = 0; j < m; ++j)if(A[i][j]==1)++hist[j];else hist[j]=0;
        int r = f(hist);
        if(r>res)res=r;
    }
    
    return res;
}
```

### Mine
```cpp
int maxUtil(int row[], int columns) {
    stack<int> result;
    int top_val;
    int max_area = 0;
    int area = 0;
    int i = 0;
    while (i < columns) {
        if (result.empty() || row[result.top()] <= row[i])
            result.push(i++);
        else {
            top_val = row[result.top()];
            result.pop();
            area = top_val * i;

            if (!result.empty())
                area = top_val * (i - result.top() - 1);
            max_area = max(area, max_area);
        }
    }
    while (!result.empty()) {
        top_val = row[result.top()];
        result.pop();
        area = top_val * i;
        if (!result.empty())
            area = top_val * (i - result.top() - 1);

        max_area = max(area, max_area);
    }
    return max_area;
}

int Solution::solve(vector<vector<int>> &mat) {
    int rows = mat.size();
    if (rows == 0) return 0;
    int columns = mat[0].size();

    int temp[columns];
    for (int i = 0; i < columns; i++) temp[i] = mat[0][i];

    int maxArea = 0;
    maxArea = max(maxArea, maxUtil(temp, columns));

    for (int i = 1; i < rows; i++) {
        for (int j = 0; j < columns; j++) temp[j] = (mat[i][j]) ? temp[j] + 1 : 0;
        maxArea = max(maxArea, maxUtil(temp, columns));
    }
    return maxArea;
}
```
### Another Mine
```cpp
int Solution::solve(vector<vector<int>> &matrix) {
    if (matrix.empty()) return 0;
    const int r = matrix.size(), c = matrix[0].size();
    int area = 0;
    vector<int> h(c + 1, 0);
    for (const auto &v : matrix) {
        for (int i = 0; i < c; ++i) h[i] = v[i] ? h[i] + 1 : 0;
        stack<int> s;
        int i = 0;
        while (i <= c) {
            if (s.empty() || h[i] >= h[s.top()])
                s.push(i++);
            else {
                int cur = s.top();
                s.pop();
                area = max(area, h[cur] * (s.empty() ? i : i - s.top() - 1));
            }
        }
    }
    return area;
}
```

