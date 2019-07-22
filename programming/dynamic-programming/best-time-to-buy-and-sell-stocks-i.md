# Best Time To Buy and Sell Stocks I

https://www.interviewbit.com/problems/best-time-to-buy-and-sell-stocks-i



## Hint1

Basically you need to find the maximum value of A[j]-A[i] where j>i.

Now can you do this?

## Hint 2

If you buy your stock on day i, you'd obviously want to sell it on the day its price is maximum after that day. 
So essentially at every index i, you need to find the maximum in the array in the suffix. 
Now this part can be done in 2 ways: 

1) Have another array which stores that information. 
max[i] = max(max[i+1], A[i])

2) Start processing entries from the end maintaining a maximum till now. Constant additional space requirement.

## Solution

```cpp

// editorial

class Solution {
    public:
        int maxProfit(vector<int> &prices) {
            int sz = prices.size();
            int maxTillNow = -1000000000, maxGain = 0;
            for (int i = sz - 1; i >= 0; i--) {
                maxGain = max(maxGain, maxTillNow - prices[i]);
                maxTillNow = max(maxTillNow, prices[i]);
            }
            return maxGain;
        }
};

// my

int Solution::maxProfit(const vector<int> &prices) {
    if(!prices.size())
        return 0;
    int res = 0, price = prices[0];
    for(int i = 1; i < prices.size(); i++){
        price = min(price, prices[i]);
        res = max(res, prices[i] - price);
    }
    return res;
}
```