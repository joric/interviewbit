# Points On The Straight Line

https://www.interviewbit.com/problems/points-on-the-straight-line



A line is made by a pair of points. 
If a,b and c lie on the same line, then line connecting a and b, and line connecting a and c will have the same slope ( (y2 - y1) / (x2 - x1)).

Make sure you handle all the corner cases.

1) Have you handled overlapping points ? 
2) Have you handled negative points for the same slope ? Like (0,0), (1,1) and (-1, -1)
3) Did you make sure that there are no divisions by 0 ? Like (1, 0), (1,4), (1, -1) 
4) How do you handle when one of the differences in x and y is 0, and the other difference is negative / positive ? Like (4, -4), (8, -4), (-4, -4)


## Solution

```cpp

// editorial

class Solution {
    public:

        int gcd(int a, int b) {
            if (a == 0) return b;
            if (b == 0) return a;
            if (a < 0) return gcd(-1 * a, b);
            if (b < 0) return gcd(a, -1 * b);
            if (a > b) return gcd(b, a);
            return gcd(b%a, a);
        }

        int maxPoints(vector<int> &X, vector<int> &Y) {
            map<pair<int, int>, int> M;
            int ans = 0;
            for (int i = 0; i < X.size(); i++) {
                M.clear();
                int same = 1, curMax = 0;
                // Try to find all the lines with same slope with points[i] as one end. 
                // Points with the same slope lie on the same line. 
                // We need to make sure we handle divisions by zero in cases like these. 
                // We also need to take care of overlapping points. 
                for (int j = i + 1; j < X.size(); j++) {
                    int xdiff = X[i] - X[j];
                    int ydiff = Y[i] - Y[j];
                    if (xdiff == 0 && ydiff == 0) {
                        // Same points 
                        same ++;
                        continue;
                    }
                    if (xdiff < 0) {
                        xdiff *= -1; 
                        ydiff *= -1;
                    }
                    int g = gcd(xdiff, ydiff);
                    M[make_pair(xdiff / g, ydiff / g)]++; 
                    curMax = max(curMax, M[make_pair(xdiff / g, ydiff / g)]);
                }
                curMax+=same;
                ans = max(ans, curMax);
            }
            return ans;
        }
};


// lightweight

struct Line {
    int x0, y0, x1, y1;
};

bool isLie(const Line& line, int px, int py) {
    int64_t lx = line.x1 - line.x0;
    int64_t ly = line.y1 - line.y0;
    if(lx == 0) return line.x0 == px;
    if(ly == 0) return line.y0 == py;
    int64_t dx = (px - line.x0) * ly;
    int64_t dy = (py - line.y0) * lx;
    return dx == dy;
}

int Solution::maxPoints(vector<int> &A, vector<int> &B) {
    int n = A.size();
    if(n <= 1) return n;
    
    int ans = 0;
    for(int i = 0; i < n; ++i) {
        for(int j = i+1; j < n; ++j) {
            Line line = { A[i], B[i], A[j], B[j] };
            int cnt = 0;
            for(int k = 0; k < n; ++k) {
                if(isLie(line, A[k], B[k])) {
                    ++cnt;
                }
            }
            ans = max(ans, cnt);
        }
    }
    return ans;
}

```