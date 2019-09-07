# Count of Range Sum

https://www.interviewbit.com/problems/count-of-range-sum/


Given an array of integers A and two integers B and C.
Find the number of range sums that lie in [ B, C ] inclusive.

Range sum S(i, j) is defined as the sum of the elements in A between indices i and j (i <= j), inclusive.


### Input Format

The argument given is the integer array A and two integers B and C.

### Output Format

Find the number of range sums that lie in  [ B, C ] inclusive.

### Constraints
```
1 <= length of the array <= 50000
-10^9 <= A[i] <= 10^9 
```

### For Example

```
Input 1:
    A = [1, 2, 3]
    B = 4
    C = 6
Output 1:
    2
    Range Sum that lie between 4 and 6 are 6(1+2+3) and 5(2+3)

Input 2:
    A = [-5, 1, -2]
    B = -2
    C = 4
Output 2:
    3
```

## Solution
### Editorial
```cpp
int mergeSort(vector<long>& b, int lower, int upper, int low, int high)
    {
        if(high-low <= 1) return 0;
        int mid = (low+high)/2, m = mid, n = mid, count =0;
        count =mergeSort(b,lower,upper,low,mid) +mergeSort(b,lower,upper,mid,high);
        for(int i =low; i< mid; i++)
        {
            while(m < high && b[m] - b[i] < lower) m++;
            while(n < high && b[n] - b[i] <= upper) n++;
            count += n - m;
        }
        inplace_merge(b.begin()+low, b.begin()+mid, b.begin()+high);
        return count;
    }

    int countRangeSum(vector<int>& a, int lower, int upper) {
        int len = a.size();
        vector<long> sum(len + 1, 0);
        for(int i =0; i< len; i++) sum[i+1] = sum[i]+a[i];
        return mergeSort(sum, lower, upper, 0, len+1);
    }

int Solution::solve(vector<int> &A, int B, int C) {
    return countRangeSum(A,B,C);
}
```

## Fastest
```cpp
long fun(vector<long> &arr, long temp[], int left, int mid, int right, int B, int C)   
{   
    int i, j, k;   
    long count = 0; 
    i = left; 
    int lowerIndex = mid; 
    int upperIndex = mid; 
    while (i <= mid - 1) {   
        while(lowerIndex <= right && (arr[lowerIndex] - arr[i] < B)) lowerIndex++; 
        while(upperIndex <= right && (arr[upperIndex] - arr[i] <= C)) upperIndex++; 
        count = count + (upperIndex - lowerIndex); 
        i++; 
    }   
    i = left;    
    j = mid;     
    k = left;  
    while ((i <= mid - 1) && (j <= right))   
    {   
        if (arr[i] <= arr[j])  
        {   
            temp[k++] = arr[i++];   
        }   
        else  
        {   
            temp[k++] = arr[j++];   
        }  
    }   
    while (i <= mid - 1)   
        temp[k++] = arr[i++];   
    while (j <= right)   
        temp[k++] = arr[j++];   
    for (i = left; i <= right; i++)   
        arr[i] = temp[i];   
    return count; 
}   
long merge(vector<long> &arr, long temp[], int l, int h, int B, int C)   
{   
    int mid;  
    long cnt = 0; 
    if(h == l)
        return arr[l] >= B && arr[l] <= C;
    if (h > l)   
    {   
        mid = (l + h) / 2;   
        cnt = merge(arr, temp, l, mid, B, C);   
        cnt += merge(arr, temp, mid + 1, h, B, C);   
        cnt += fun(arr, temp, l, mid + 1, h, B, C);   
    }   
    return cnt;   

} 
int Solution::solve(vector<int> &v, int B, int C) {
    long int cnt = 0, i, j, x;
    long temp[v.size() - 1];
    int len=v.size();
    vector<long> pre(len, 0);
    pre[0] = v[0];
    for(i = 1; i < len; i++){
        pre[i] = pre[i - 1] + v[i];
    }
    return merge(pre, temp, 0, pre.size() - 1, B, C);
}
```

### Lightweight
```cpp
int Solution::solve(vector<int> &A, int B, int C) {
     #define ll long long 
    int n = A.size();
    ll sum,x,ans=0;
    for(int i=0;i<n;i++){
        sum=0;
        for(int j=i;j<n;j++){
            x = A[j]; 
            sum += x;
            if(sum>=B&&sum<=C) ans++;
        }
    }
    return ans;
}
```


### Mine
```cpp
int mergeSort(vector<long>& sum, int lower, int upper, int low, int high) {
    if (high-low <= 1) return 0;
    int mid = (low+high)/2, m = mid, n = mid, count =0;
    count = mergeSort(sum,lower,upper,low,mid) + mergeSort(sum,lower,upper,mid,high);
    for(int i = low;i < mid; ++i){
        auto m = lower_bound(sum.begin()+mid,sum.begin()+high,sum[i] + lower);
        auto n = upper_bound(sum.begin()+mid,sum.begin()+high,sum[i] + upper);
        count += n-m;
    }
    inplace_merge(sum.begin()+low, sum.begin()+mid, sum.begin()+high);
    return count;
}
int Solution::solve(vector<int>& nums, int lower, int upper) {
    int len = nums.size();
    vector<long> sum(len + 1, 0);
    for(int i=0; i<len; i++) sum[i+1] = sum[i]+nums[i];
    return mergeSort(sum, lower, upper, 0, len+1);
}
```
