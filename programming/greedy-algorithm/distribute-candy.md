# Distribute Candy

https://www.interviewbit.com/problems/candy



## Hint 1

Would greedily assigning the candy work here ? 
Where should you start from ? 
Should you start assigning the candies from the least rating guy to the guy with most rating or vice versa ?

## Hint 2

Greedy works here ( Think of a supportive proof as as assignment ).

Start with the guy with the least rating. Obviously he will receive 1 candy. 
If he did recieve more than one candy, we could lower it to 1 as none of the neighbor have higher rating. 
Now lets move to the one which is second least. If the least element is its neighbor, then it receives 2 candies, else we can get away with assigning it just one candy. 
We keep repeating the same process to arrive at optimal solution.
## Solution

```cpp


int Solution::candy(vector<int> &ratings) {
    int candies = 0;
    int size = ratings.size();
    vector<int> left(size, 1);
    vector<int> right(size, 1);
    
    for (auto l = 1; l<size; ++l)
        if (ratings[l] > ratings[l-1])
            left[l] = left[l-1] + 1;
    
    for (auto r = size-2; r>=0; --r)
        if (ratings[r] > ratings[r+1])
            right[r] = right[r+1] + 1;
            
    for (auto i = 0; i<size; ++i)
        candies += max(left[i], right[i]);
        
    return candies;
}

//fastest 

int Solution::candy(vector<int> &ratings) {

    int sum=0;
    int *arr = new int[ratings.size()];

    for(int i=0;i<ratings.size();i++){

        if(i>0&&ratings[i]>ratings[i-1])
            arr[i]=arr[i-1]+1;
        else
            arr[i]=1;

        int j=i;

        while(j>0&&arr[j]>=arr[j-1]&&ratings[j]<ratings[j-1])
            arr[j-1]++, j--;
    }

    for(int i=0;i<ratings.size();i++)
        sum+=arr[i];

    return sum;
}

// editorial

class Solution {
public:
    int candy(vector<int> &ratings) {
        int candiesCnt = 0, curCandy, pos;
        vector<pair<int, int>> valueWithPos;
        for (int i = 0; i < ratings.size(); i++) {
            valueWithPos.push_back(make_pair(ratings[i], i));
        }
        sort(valueWithPos.begin(), valueWithPos.end());
        vector<int> candies(ratings.size(), 0);
        for (int i = 0; i < valueWithPos.size(); i++) {
            pos = valueWithPos[i].second;
            curCandy = 0;
            if (pos > 0 && ratings[pos - 1] < ratings[pos]) {
                curCandy = candies[pos - 1];
            }
            if (pos < ratings.size() - 1 && ratings[pos + 1] < ratings[pos]) {
                curCandy = max(curCandy, candies[pos + 1]);
            }
            candies[pos] = curCandy + 1;
            candiesCnt += candies[pos];
        }
        return candiesCnt;
    }
};

// simple-linear

int Solution::candy(vector<int> &A) {
    int n = A.size();
    int cand[n];
    fill(cand, cand + n, 1);

    for (int i = 1; i < n; i++) {
        if (A[i] > A[i - 1]) {
            cand[i] = cand[i - 1] + 1;
        }
    }

    for (int i = n - 2; i >= 0; i--) {
        if (A[i] > A[i + 1]) {
            cand[i] = max(cand[i], cand[i + 1] + 1);
        }
    }

    return accumulate(cand, cand + n, 0);
}

//////////////

int Solution::candy(vector<int> &ratings) {
    int candies = 0;
    int size = ratings.size();
    vector<int> lefttoright(size, 1);
    vector<int> righttoleft(size, 1);

    for (auto l = 1; l < size; ++l)
        if (ratings[l] > ratings[l - 1])
            lefttoright[l] = lefttoright[l - 1] + 1;

    for (auto r = size - 2; r >= 0; --r)
        if (ratings[r] > ratings[r + 1])
            righttoleft[r] = righttoleft[r + 1] + 1;

    for (auto i = 0; i < size; ++i)
        candies += max(lefttoright[i], righttoleft[i]);

    return candies;
}
```