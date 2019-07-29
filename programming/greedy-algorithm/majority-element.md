# Majority Element

https://www.interviewbit.com/problems/majority-element




## Hint 1

Lets say you find 2 elements x and y which are different in the array.
If you removed those 2 elements, would the majority element change ?

## Hint 2

If we cancel out each occurrence of an element e with all the other elements
that are different from e then e will exist till end if it is a majority element.
Loop through each element and maintains a count of the element that has the potential
of being the majority element. If next element is same then increments the count,
otherwise decrements the count. If the count reaches 0 then update the potential
index to the current element and reset count to 1.


## Solution

### Editorial
```cpp
int Solution::majorityElement(vector<int> &num) {
    int majorityIndex = 0;
    for (int count = 1, i = 1; i < num.size(); i++) {
        num[majorityIndex] == num[i] ? count++ : count--;
        if (count == 0) {
            majorityIndex = i;
            count = 1;
        }
    }
    return num[majorityIndex];
}

```
### Fastest
```cpp
int Solution::majorityElement(const vector<int> &A) {
    int n=A.size();
    int maxi=0,count=1;
    for(int i=1;i<n;i++){
        if(A[i]!=A[maxi]){
            count--;
            if(count==0){
                count=1;
                maxi=i;
            }
        }else{
            count++;
        }
    }
    return A[maxi];
}
```

### Lightweight
```cpp
int Solution::majorityElement(const vector<int> &A) {
    for(int i=0; i<A.size(); i++){
        int cnt = 0;
        for(int j=0; j<A.size(); j++){
            if(A[i] == A[j])
                cnt++;
            else
                cnt--;
        }
        if(cnt>0)
            return A[i];
    }
}
```

### Mine
```cpp
int Solution::majorityElement(const vector<int> &A) {
    // boyer-moore majority vote
    int x = -1, c = 0;
    for (int i = 0; i < n; i++) if (c == 0) x = a[i], c++; else c += x == a[i] ? 1 : -1;
    return x;
}

```