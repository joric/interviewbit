# Seats

https://www.interviewbit.com/problems/seats/

## Hint 1

Hint: Is it possible to use the median position somehow ?

Are you getting "Wrong Answer" on time complexity check ? Did you miss taking mod from MOD = 10000003

## Hint 2

Lets take an exmaple:

string:  .x..x..x.
positions where x are present {1, 4, 7}
Median is 4. So we will move all x near our median.
1st person would need to jump 2 steps and 3rd person would also need to jump 2 steps. Total answer = 4.
Can you prove why this approach works ?

Happy Coding

## Solution

```cpp

////////////
// editorial

#define MOD 10000003

class Solution {
public:
    int seats(string str) {

        int N = str.size();

        vector<int> Pos;
        for (int i = 0; i < N; ++i) {

            if (str[i] == 'x') {
                Pos.push_back(i);
            }
        }

        int n = Pos.size();

        if (n == 0)
            return 0;

        int mid = n / 2;
        int median = n & 1 ? Pos[mid] : (Pos[mid] + Pos[mid - 1]) / 2;

        int ans = 0;

        int prev = median;
        int empty = str[median] == 'x' ? median - 1 : median;
        for (int i = median - 1; i >= 0; --i) {
            if (str[i] == 'x') {
                ans += empty - (i);

                if (ans >= MOD)
                    ans %= MOD;

                prev = empty;
                empty--;
            }
        }

        prev = median;
        empty = median + 1;
        for (int i = median + 1; i < N; ++i) {
            if (str[i] == 'x') {
                ans += (i)-empty;
                if (ans >= MOD)
                    ans %= MOD;
                prev = empty;
                empty++;
            }
        }

        return ans;
    }
};

//////////////////
// fastest
unsigned long long seats(std::string const &row) {
    unsigned long long sum = 0;

    std::size_t l = 0;
    std::size_t r = row.size();
    while (l < r && row[l] != 'x') {
        ++l;
    }
    while (l < r && row[r - 1] != 'x') {
        --r;
    }

    std::size_t lsize = 0;
    while (l < r && row[l] == 'x') {
        ++lsize;
        ++l;
    }
    std::size_t rsize = 0;

    bool count_left = false;

    unsigned long long moves = 0;

    while (l < r) {
        if (count_left) {
            auto const lstart = l;
            while (l < r && row[l] == 'x') {
                ++l;
            }
            lsize += l - lstart;
        } else {
            auto const rend = r;
            while (l < r && row[r - 1] == 'x') {
                --r;
            }
            rsize += rend - r;
        }

        count_left = lsize < rsize;

        if (count_left) {
            auto const lgapstart = l;
            while (l < r && row[l] != 'x') {
                ++l;
            }
            auto const lgap = l - lgapstart;
            moves += lgap * lsize;
        } else {
            auto const rgapend = r;
            while (l < r && row[r - 1] != 'x') {
                --r;
            }
            auto const rgap = rgapend - r;
            moves += rgap * rsize;
        }
    }

    return moves;
}

int Solution::seats(string row) {
    return ::seats(row) % 10000003;
}

///////////////
// lightweight

// Do not write main() function.
// Do not read input, instead use the arguments to the function.
// Do not print the output, instead return values as specified
// Still have a doubt. Checkout www.interviewbit.com/pages/sample_codes/ for more details
/**
 * @input A : String termination by '\0'
 * 
 * @Output Integer
 */
long long mod(long long a, long long b) {
    while (a < 0) {
        a = a + b;
    }
    return a;
}

int Solution::seats(string s) {
    const char *A = s.c_str();
    long long i, j = 0;
    long long sum = 0;
    long long m = 10000003;
    long long len = 0, count = 0;
    for (i = 0; i < strlen(A); i++) {
        if (A[i] == 'x')
            //        temp[j++] = i;
            len++;
    }

    /*  for(i=0;i<j/2;i++){
        sum = (sum + temp[j-1-i] - temp[i])%m;
    }*/
    long long b = len / 2;
    if (len % 2 == 1) {
        b = len / 2 + 1;
    }

    for (i = 0; i < strlen(A); i++) {
        if (A[i] == 'x') {
            if (count < len / 2) {
                sum = mod(sum - i, m) % m;
            }
            if (count >= b) {
                sum = (sum + i + m) % m;
            }
            count++;
        }
    }

    sum = mod((sum - (len / 2) * (len / 2 + 1)), m) % m;

    if (len % 2 == 1)
        return (sum + m) % m;
    else
        return (sum + m + len / 2) % m;
}

////////////////
// my

int Solution::seats(string A) {
    // Do not write main() function.
    // Do not read input, instead use the arguments to the function.
    // Do not print the output, instead return values as specified
    // Still have a doubt. Checkout www.interviewbit.com/pages/sample_codes/ for more details

    int count = 0;

    for (int i = 0; i < A.size(); i++) {
        if (A[i] == 'x') {
            count++;
        }
    }

    if (count == 0 || count == 1) {
        return 0;
    }

    int mid = 0;

    for (int i = 0; i < A.size(); i++) {
        if (A[i] == 'x') {
            mid++;
            if (mid == (count / 2) + 1) {
                mid = i;
                break;
            }
        }
    }

    long long int i = 0, j = A.size() - 1, l = mid - 1, r = mid + 1;
    long long int sol = 0;

    while (i < l || j > r) {
    LOO:
        if (i < l) {
            if (A[i] == 'x') {
                if (A[l] == 'x') {
                    l--;
                    goto LOO;
                }
                sol = sol + l - i;
                sol = sol % 10000003;
                char temp = A[i];
                A[i] = A[l];
                A[l] = temp;
                l--;
            }
        }
    LOOP:
        if (j > r) {
            if (A[j] == 'x') {
                if (A[r] == 'x') {
                    r++;
                    goto LOOP;
                }
                sol = sol - r + j;
                sol = sol % 10000003;
                char temp = A[j];
                A[j] = A[r];
                A[r] = temp;
                r++;
            }
        }
        j--;
        i++;
    }

    // for(int i = 0; i < A.size(); i++){
    //     cout << A[i];
    // }

    return (int)(sol % 10000003);
}
```