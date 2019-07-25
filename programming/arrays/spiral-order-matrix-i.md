# Spiral Order Matrix I

https://www.interviewbit.com/problems/spiral-order-matrix-i/

Given a matrix of m * n elements (m rows, n columns), return all elements of the matrix in spiral order.

Example:

Given the following matrix:

[
    [ 1, 2, 3 ],
    [ 4, 5, 6 ],
    [ 7, 8, 9 ]
]
You should return

[1, 2, 3, 6, 9, 8, 7, 4, 5]

Problem Approach :

Complete solution in the hints.

## Solution
### Editorial
```cpp
vector<int> Solution::spiralOrder(const vector<vector<int>> &matrix) {
    int rows = matrix.size();
    if (rows == 0) return vector<int>();
    int cols = matrix[0].size();
    int row = 0, col = 0, layer = 0;
    vector<int> res;
    res.push_back(matrix[0][0]);
    int dir = 1;
    for (int step = 1; step < rows * cols; step++) {
        switch (dir) {                     // based on dir, check and change dir, then move on
        case 1:                            // supposed to go right
            if (col == cols - layer - 1) { // reach right bound
                row++;
                dir = 2;
            } else
                col++;
            break;
        case 2:                            // supposed to go down
            if (row == rows - layer - 1) { // reach downside bound
                col--;
                dir = 3;
            } else
                row++;
            break;
        case 3:                 // supposed to go left
            if (col == layer) { // reach left bound
                row--;
                dir = 4;
            } else
                col--;
            break;
        case 4:                     // supposed to go up
            if (row == layer + 1) { // reach upside bound
                col++;
                dir = 1;
                layer++;
            } else
                row--;
            break;
        }
        res.push_back(matrix[row][col]);
    }
    return res;
}
```

### Fastest
```cpp
vector<int> Solution::spiralOrder(const vector<vector<int> > &A) {
	vector<int> result;
	// DO STUFF HERE AND POPULATE result
	
	int t,b,l,r,dir,i ;
	t = 0;
	b = A.size() - 1;
	l = 0;
	dir = 0 ;
	r = A[0].size() - 1;
	while ( t<=b && l<=r)
	{
	    if (dir == 0)
	    {
	        for (i=l;i<=r;i++)
	            result.push_back(A[t][i]) ;
	        t++;
	    }
	    else if (dir == 1)
	    {
	        for (i=t;i<=b;i++)
	            result.push_back(A[i][r]) ;
	        r--;
	    }
	    else if (dir == 2)
	    {
	        for (i=r;i>=l;i--)
	            result.push_back(A[b][i]) ;
	        b--;
	    }
	    else if (dir == 3)
	    {
	        for (i=b;i>=t;i--)
	            result.push_back(A[i][l]) ;
	        l++;
	    }
	    dir = ( dir + 1 )%4 ;
	}
	
	return result;
}

```

### Lightweight
```cpp
vector<int> Solution::spiralOrder(const vector<vector<int> > &A) {
	vector<int> result;
	int T=0,B,L=0,R,dir=0,i,j;
	B=A.size()-1;
	R=A[0].size()-1;
	while(T<=B && L<=R)
	{ if(dir==0){
	    for(i=L;i<=R;i++)
	    cout<<A[T][i]<<" ";
	    T++;
	    dir=1;
	           }
	   else if(dir==1)
	   { for(i=T;i<=B;i++)
	        cout<<A[i][R]<<" ";
	            R--;
	            dir=2;
	   }
	   else if(dir==2)
	   {    for(i=R;i>=L;i--)
	            cout<<A[B][i]<<" ";
	            B--;
	            dir=3;
	    }
	    else if(dir==3) {
	            for(i=B;i>=T;i--)
	            cout<<A[i][L]<<" ";
	            L++;
	            dir=0;
	    }
	}
	
	return result;
}
```

### Other
```cpp
vector<int> Solution::spiralOrder(const vector<vector<int> > &A) {
    int rows = A.size();
    if (rows == 0)
        return vector<int>();
    int cols = A[0].size();
    vector<int> result;
    result.resize(rows*cols);
    // DO STUFF HERE AND POPULATE result
    int l = 0, t = 0, r = cols-1, b = rows-1;
    int dir = 0;
    int n = -1;
    while (t<=b && l<=r)
    {
        switch (dir)
        {
            case 0:
                for (auto i = l; i<=r; ++i)
                    result[++n] = A[t][i];
                ++t; dir = 1;
                break;
            case 1:
                for (auto i = t; i<=b; ++i)
                    result[++n] = A[i][r];
                --r; dir = 2;
                break;
            case 2:
                for (auto i = r; i>=l; --i)
                    result[++n] = A[b][i];
                --b; dir = 3;
                break;
            case 3:
                for (auto i = b; i>=t; --i)
                    result[++n] = A[i][l];
                ++l; dir = 0;
                break;
        }
    }
    return result;
}

```


## Asked in
* Microsoft
* JP Morgan
* Amazon
* Flipkart
