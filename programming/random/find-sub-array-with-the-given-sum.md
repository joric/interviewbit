# Find sub-array with the given sum

https://www.interviewbit.com/problems/find-sub-array-with-the-given-sum/


Given an array of positive integers A and an integer B,
find and return first continuous subarray which adds to B.

If the answer does not exist return an array with a single element “-1”.

Note: First sub-array means the sub-array for which starting index in minimum.

### Input Format

The first argument given is the integer array A.

The second argument given is integer B.

### Output Format

Return the first continuous sub-array which adds to B and if the answer does not exist return an array with a single element "-1".

### Constraints

```
1 <= length of the array <= 100000
1 <= A[i] <= 10^9 
1 <= B <= 10^9
```

### For Example

```
Input 1:
    A = [1, 2, 3, 4, 5]
    B = 5
Output 1:
    [2, 3]

Input 2:
    A = [5, 10, 20, 100, 105]
    B = 110
Output 2:
    [-1]
```

## Solution
### Editorial
```cpp
vector<int> Solution::solve(vector<int> &A, int B) {
    int n = A.size();
    int l = 0, r = 0;
    long long int sum = A[l];
    while (r < n && l <= r)
    {
        if (sum == B)
        {
            vector <int> ans;
            for (int i = l; i <= r; i++)    ans.push_back(A[i]);
            return ans;
        }
        else if (sum < B)
        {
            r++;
            if (r < n)  sum += A[r];
        }
        else
        {
            sum -= A[l];
            l++;
        }
    }
    return {-1};
}
```

### Fastest
```cpp
vector<int> Solution::solve(vector<int> &A, int X) {
    int sum = 0, end = -1, start = 0;
    bool found = false;
    vector<int> v;
    
    for(int i = 0;i<A.size();){
        //cout<<" i1 = "<<i<<endl;
        if(A[i]>X){
            sum = 0;
            start = i+1;
            end = i;
            i++;
            continue;
        }
        
        while(i<A.size() && sum<X){
            //cout<<" i2 = "<<i<<endl;
            sum += A[i];
            i++;
        }
        end = i-1;
        
        int left = start;
        while(sum > X){
            //cout<<" i3 = "<<left<<endl;
            sum -= A[left];
            left++;
        }
        start = left;
        
        if(sum==X){
            found = true;
            break;
        }    
    }
    
    if(!found){
        v.push_back(-1);
        return v;
    }
    for(int i=start;i<=end;i++)
        v.push_back(A[i]);
    return v;
}
```
### Lightweight
```cpp
 vector<int> Solution::solve(vector<int> &A, int B) {
        int n=A.size();
        vector<int>v;
        int i=0,j=0;
        int sum=A[i];
        // cout<<"hello"<<endl;
        while(i<n&&j<n){
            
            if(sum>B){
                if(i==j){
                    i++;
                    j++;
                    sum=A[i];
                }else{
                    
                    sum-=A[i];
                    i++;
                }
            }else if(sum<B){
                j++;
                sum+=A[j];
            }else{
                // cout<<"heyy"<<endl;
                // cout<<i<<" "<<j<<endl;
                for(int k=i;k<=j;k++){
                    v.push_back(A[k]);
                }
                // cout<<"bye"<<endl;
                break;
                
            }
        }
        if(v.size()==0){
            v.push_back(-1);
        }
        return v;
    }
```

### Mine
```cpp
vector<int> Solution::solve(vector<int> &arr, int sum) {
    int windowSum = 0, n = arr.size();
    for (int i=0, j=0; i<n; i++) {
        while (j < n && windowSum < sum)
            windowSum += arr[j++];
        if (windowSum == sum) {
            vector<int> res(j-i);
            copy(arr.begin()+i, arr.begin()+j, res.begin());
            return res;
        }
        windowSum -= arr[i];
    }
    return {-1};
}
```
