# Allocate Books

https://www.interviewbit.com/problems/allocate-books



N number of books are given. 
The ith book has Pi number of pages. 
You have to allocate books to M number of students so that maximum number of pages alloted to a student is minimum. A book will be allocated to exactly one student. Each student has to be allocated at least one book. Allotment should be in contiguous order, for example: A student cannot be allocated book 1 and book 3, skipping book 2.

NOTE: Return -1 if a valid assignment is not possible

Input:

List of Books
M number of students
Your function should return an integer corresponding to the minimum number.

Example:

P: [12, 34, 67, 90]
M: 2

Output: 113

There are 2 number of students. Books can be distributed in following fashion: 
  1) [12] and [34, 67, 90]
      Max number of pages is allocated to student 2 with 34 + 67 + 90 = 191 pages
  2) [12, 34] and [67, 90]
      Max number of pages is allocated to student 2 with 67 + 90 = 157 pages 
  3) [12, 34, 67] and [90]
      Max number of pages is allocated to student 1 with 12 + 34 + 67 = 113 pages

Of the 3 cases, Option 3 has the minimum pages = 113. 

## Hint 1

Can you find how many number of students we need if we fix that one student can read atmost V number of pages ?

## Hint 2

Problem statement: Given fixed number of pages (V),  how many number of students we need?
Solution :
   simple simulation approach
   intially Sum := 0
   cnt_of_student = 0
   iterate over all books:
        If Sum + number_of_pages_in_current_book > V :
                  increment cnt_of_student
                  update Sum
        Else:
                  update Sum
   EndLoop;
  


    fix range LOW, HIGH
    Loop until LOW < HIGH:
            find MID_point
            Is number of students required to keep max number of pages below MID < M ? 
            IF Yes:
                update HIGH
            Else
                update LOW
    EndLoop;


## Solution

```cpp

/* editorial */

int split(vector<long> &sum, long target) {
    int cnt=0;
    auto it=sum.begin()+1;
    while (it!=sum.end()) {
        it--;
        it=upper_bound(it+1,sum.end(),*it+target);
        cnt++;
    }
    return cnt;
}
int Solution::books(vector<int> &A, int B) {
    if (A.empty() || A.size()<B) return -1;
    vector<long> sum(A.size()+1,0);
    long lo(INT_MIN), hi;
    for (int i=0; i<A.size(); ++i) {
        lo=max(lo,(long)A[i]);
        sum[i+1]=A[i]+sum[i];
    }
    hi=sum.back();
    while (lo<hi) {
        long mid = lo+(hi-lo)/2;
        if (split(sum,mid)>B) lo=mid+1;
        else hi=mid;
    }
    return lo;
}


/* --- */

bool isPossible(const vector<int> A, int K, long long mid)
{
    long long total = 0; int cnt = 1;
    for (auto i = 0; i<A.size();)
    {
        if((long long)A[i] > mid) return false;
        if(total + (long long)A[i] > mid)
        {
            total = 0;
            cnt++;
        }
        else
        {
            total += (long long)A[i];
            i++;
        }
    }
    
    if(cnt <= K)
        return true;
    return false;
}
int Solution::books(vector<int> &A, int B) {
    if(A.size() < B) //Corner case
        return -1;
    long long low = *max_element(A.begin(), A.end());
    long long high = 0;
    for(auto i = 0; i<A.size(); ++i)
        high += (long long)A[i];
    long long ans = INT_MAX;
    ans *= INT_MAX;
    while (low<=high)
    {
        long long mid = low + (high - low)/2;
        if (isPossible(A, B, mid))
        {
            ans = min(ans, mid);
            high = mid - 1;
        }
        else
            low = mid + 1;
    }
    return ans%INT_MAX;
}
```