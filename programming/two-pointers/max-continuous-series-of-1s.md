# Max Continuous Series Of 1s

https://www.interviewbit.com/problems/max-continuous-series-of-1s


Max Continuous Series of 1s

You are given with an array of 1s and 0s. And you are given with an integer M, which signifies number of flips allowed.
Find the position of zeros which when flipped will produce maximum continuous series of 1s.

For this problem, return the indices of maximum continuous series of 1s in order.

Example:

Input: 
Array = {1 1 0 1 1 0 0 1 1 1 } 
M = 1

Output: 
[0, 1, 2, 3, 4] 

If there are multiple possible solutions, return the sequence which has the minimum start index.



N: 4
lis: 1 0 1 0 
M: 2

pointer i and j 
i = j = 0
iterate till i < N:
        if(Number_of_Zeros_in_Current_range > M) :
                increment j and reduce range till Number_of_Zeros_in_current_range < M
        else :
                add element in range and update all variables

## Solution

```cpp


/* my solution */

vector<int> Solution::maxone(vector<int> &arr, int m) {
    int n = arr.size();
    int i = 0, j = 0;
    int ofs = 0, w = 0;
    int zeros = 0;

    while (j < n) {
        if (zeros <= m) {
            if (arr[j] == 0) zeros++;
            j++;
        }
        if (zeros > m) {
            if (arr[i] == 0) zeros--;
            i++;
        }
        if (j - i > w) {
            w = j - i;
            ofs = i;
        }
    }

    vector<int> best(w);
    iota(best.begin(), best.end(), ofs);
    return best;
}

/* editorial */
vector<int> Solution::maxone(vector<int> &Vec, int M) {
    int N = Vec.size();
    int i = 0;
    int j = 0;
    int max_len = 0;
    int cnt = 0;
    vector<int> ans;

    int ans_start = 0;
    int ans_end = 0;

    while (i < N) {
        if (cnt + (Vec[i] == 0) > M) {
            int temp_len = (i - j); // length from j to i - 1.
            if (temp_len > max_len) {
                max_len = temp_len;
                ans_start = j;
                ans_end = i - 1;
            }

            while (i >= j && cnt + (Vec[i] == 0) > M) {
                cnt -= (Vec[j] == 0);
                j++;
            }
            cnt += (Vec[i] == 0);
        } else {
            cnt += (Vec[i] == 0);
        }

        i++;
    }

    int temp_len = (i - j);
    if (temp_len > max_len) {
        temp_len = max_len;
        ans_start = j;
        ans_end = i - 1;
    }

    for (int ele = ans_start; ele <= ans_end; ele++) {
        ans.push_back(ele);
    }
    return ans;
}

/* another solution */


vector<int> Solution::maxone(vector<int> &A, int B) {
    // Do not write main() function.
    // Do not read input, instead use the arguments to the function.
    // Do not print the output, instead return values as specified
    // Still have a doubt. Checkout www.interviewbit.com/pages/sample_codes/ for more details

    vector<int> sol;

    int i = 0, j = 0, temp_st = -1, temp_end = -1, temp_count = 0;
    int ov_st = -1, ov_end = -1, ov_count = 0;

    int temp = B;

    while (j < A.size()) {
        if (A[j] == 1) {
            temp_count++;
            if (temp_count == 1) {
                temp_st = j;
            }
            temp_end = j;
            j++;
        } else {
            temp--;
            if (temp < 0) {
                int t_c = 1;
                while (A[i] == 1) {
                    i++;
                    t_c++;
                }
                i++;
                temp_count = temp_count - t_c + 1;
                temp = 0;
                temp_st = i;
                temp_end = j;
            } else {
                temp_count++;
                if (temp_count == 1) {
                    temp_st = j;
                }
                temp_end = j;
            }
            j++;
        }
        if (ov_count == 0) {
            ov_count = temp_count;
            ov_st = temp_st;
            ov_end = temp_end;
        } else {
            if (ov_count < temp_count) {
                ov_count = temp_count;
                ov_st = temp_st;
                ov_end = temp_end;
            }
        }
    }

    if (ov_count != 0) {
        for (int t = ov_st; t <= ov_end; t++) {
            sol.push_back(t);
        }
    }

    return sol;
}

/* editorial - fastest */

vector<int> Solution::maxone(vector<int> &arr, int m) {
    int n = arr.size();
    // Left and right indexes of current window
    int wL = 0, wR = 0;

    // Left index and size of the widest window
    int bestL = 0, bestWindow = 0;

    // Count of zeroes in current window
    int zeroCount = 0;

    // While right boundary of current window doesn't cross
    // right end
    while (wR < n) {
        // If zero count of current window is less than m,
        // widen the window toward right
        if (zeroCount <= m) {
            if (arr[wR] == 0)
                zeroCount++;
            wR++;
        }

        // If zero count of current window is more than m,
        // reduce the window from left
        if (zeroCount > m) {
            if (arr[wL] == 0)
                zeroCount--;
            wL++;
        }

        // Updqate widest window if this window size is more
        if (wR - wL > bestWindow) {
            bestWindow = wR - wL;
            bestL = wL;
        }
    }
    vector<int> best;
    // Print positions of zeroes in the widest window
    for (int i = 0; i < bestWindow; i++) {
        best.push_back(bestL + i);
    }
    return best;
    // Do not write main() function.
    // Do not read input, instead use the arguments to the function.
    // Do not print the output, instead return values as specified
    // Still have a doubt. Checkout www.interviewbit.com/pages/sample_codes/ for more details
}
```