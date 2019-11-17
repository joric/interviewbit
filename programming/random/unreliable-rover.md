# Unreliable Rover

https://www.interviewbit.com/problems/unreliable-rover/

Given a string A of size N describing the movement of a rover in the infinite matrix.
Fortunately we know the initial position of the rover in the matrix.You need to locate the final position of the rover.
Infinite matrix is divided into squares with side length 1 meter.
In this infinite matrix, In one step rover moves onto the next square in one of the four directions(North, South, East or West).

If:

A[i] == 'N' Rover moves in the North direction

A[i] == 'S' Rover moves in the South direction

A[i] == 'E' Rover moves in the East direction

A[i] == 'W' Rover moves in the West direction

A[i] == '?' we don't know in which direction Rover is moving. It can be any of 'N', 'S', 'E' or 'W'.

Two integer arrays B and C are given denoting the minimum and maximum steps taken by the rover
in A[i]th direction.

Find and return the area (in square meters) of the region where Rover can currently be located.

Since this area can be large return area modulo (109+7).

Note: Inital position of the rover does not change the answer.You can assume any initial position of the rover.


Input Format

The only argument given is the integer array A.
Output Format

Return the area modulo (10^9+7).
Constraints

1 <= N <= 50
A[i] = {'N', 'S', 'E', 'W', '?'};
'?' can occur atmost 20 times in A
0 <= B[i] <= C[i] <= 10^7
if A[i] == '?':
    B[i] is 0
For Example

Input 1:
    A = "NE"
    B = [3, 3]
    C = [5, 5]

Output 1:
    9
    Conidering initial position of rover as (0, 0).
    The rover ended at one of the cells (x,y) witn 3 <= x,y <= 5.


Input 2:
    A = "?"
    B = [0]
    C = [2]
Output 2:
    9
    Conidering initial position of rover as (0, 0).
    The rover can end at any of the squares (-2,0), (-1,0), (0,-2), (0,-1), (0,0), (0,1), (0,2), (1,0), or (2,0).

## Solution

### Editorial
```cpp
const long long MOD = 1000000007;

int getArea(string &direction, vector <int> &minSteps, vector <int> &maxSteps) {
  int n = (int) direction.size();
  vector<int> unknown;
  int sx = 1, sy = 1;
  for (int i = 0; i < n; i++) {
    if (direction[i] == '?') {
      unknown.push_back(maxSteps[i]);
    } else {
      if (direction[i] == 'N' || direction[i] == 'S') {
        sy += maxSteps[i] - minSteps[i];
      } else {
        sx += maxSteps[i] - minSteps[i];
      }
    }
  }
  vector<int> sums;
  int sz = (int) unknown.size();
  for (int t = 0; t < (1 << sz); t++) {
    int sum = 0;
    for (int i = 0; i < sz; i++) {
      if (t & (1 << i)) {
        sum += unknown[i];
      }
    }
    sums.push_back(sum);
  }
  sort(sums.begin(), sums.end());
  int total = sums.back();
  long long ans = (long long) sx * sy + (long long) sx * total * 2 + (long long) sy * total * 2;
  ans += (long long) 2 * total * (total - 1);
  for (int i = 0; i < (int) sums.size() - 1; i++) {
    int cur = sums[i + 1] - sums[i];
    ans -= (long long) 2 * cur * (cur - 1);
  }
  return (ans%MOD);
}


int Solution::solve(string A, vector<int> &B, vector<int> &C) {
    return getArea(A,B,C);
}
```

### Lightweight
```cpp
int Solution::solve(string A, vector<int> &B, vector<int> &C) {
    int sx = 1 , sy = 1 ;
    vector<int> chk ;
    for(int i =  0 ; i < A.length() ; i++)
    {
        if(A[i]=='?')
        {
            chk.push_back(C[i]);
        }
        else
        {
            if(A[i]=='N' || A[i]=='S')
            {
                sy = sy + C[i]-B[i] ;
            }
            else
            {
                sx = sx+C[i]-B[i];
            }
        }
    }
    
    int sz = chk.size() ;
    vector<int> sums ;
    for(int t = 0 ; t < (1<<sz) ; t++)
    {
        int sum = 0 ;
        for(int i = 0 ; i < sz ; i++)
        {
            if(t & (1 << i))
            {
                sum = sum + chk[i];
            }
        }
        sums.push_back(sum);
        
    }
    
    sort(sums.begin() , sums.end());
    int total = sums.back() ;
    
    long long int ans = 0 ;
    ans = (long long)sx*sy + (long long)2*total*sx + (long long)2*total*sy ;
    ans = ans + (long long)2*total*(total-1);
    
    for(int i = 0 ; i < sums.size()-1 ; i++)
    {
        int curr = sums[i+1] - sums[i] ;
        ans = ans - (long long)2*(curr)*(curr-1);
    }
    
    return ans%1000000007 ;
    
    
}
```

