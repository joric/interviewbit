# Gas Station

https://www.interviewbit.com/problems/gas-station

There are N gas stations along a circular route, where the amount of gas at station i is gas[i].

You have a car with an unlimited gas tank and it costs cost[i] of gas to travel from station i to its next station (i+1). You begin the journey with an empty tank at one of the gas stations.

Return the minimum starting gas station's index if you can travel around the circuit once, otherwise return -1.

You can only travel in one direction. i to i+1, i+2, ... n-1, 0, 1, 2..
Completing the circuit means starting at i and ending up at i again.

Example :

Input :
      Gas:   [1, 2]
      Cost:  [2, 1]

Output: 1 

If you start from index 0, you can fill in gas[0] = 1 amount of gas. Now your tank has 1 unit of gas. But you need cost[0] = 2 gas to travel to station 1. 
If you start from index 1, you can fill in gas[1] = 2 amount of gas. Now your tank has 2 units of gas. You need cost[1] = 1 gas to get to station 0. So, you travel to station 0 and still have 1 unit of gas left over. You fill in gas[0] = 1 unit of additional gas, making your current gas = 2. It costs you cost[0] = 2 to get to station 1, which you do and complete the circuit. 


## Hint 1

Try to find the relation between sum of gas and sum of cost for solution to exist.

When will you shift your starting point from 0? 
Do you really need to cover every starting point? How can you use answer of above question to optimize this part?

## Hint 2

The bruteforce solution should be obvious. Start from every i, and check to see if every point is reachable with the gas available. Return the first i for which you can complete the trip without the gas reaching a negative number. 
This approach would however be quadratic.

Lets look at how we can improve. 
1) If sum of gas is more than sum of cost, does it imply that there always is a solution ? 
2) Lets say you start at i, and hit first negative of sum(gas) - sum(cost) at j. We know TotalSum(gas) - TotalSum(cost) > 0. What happens if you start at j + 1 instead ? Does it cover the validity clause for i to j already ?


## Solution

```cpp

/////////////////
// editorial

int Solution::canCompleteCircuit(const vector<int> &gas, const vector<int> &cost) {
    int sumGas = 0;
    int sumCost = 0;
    int start = 0;
    int tank = 0;

    for (int i = 0; i < gas.size(); i++) {
        sumGas += gas[i];
        sumCost += cost[i];
        tank += gas[i] - cost[i];
        if (tank < 0) {
            start = i + 1;
            tank = 0;
        }
    }

    return sumGas < sumCost ? - 1 : start;
}

/////////////////
// my

int Solution::canCompleteCircuit(const vector<int> &gas, const vector<int> &cost) {

    int fuel = 0, start_i = 0, sum = 0;

    for (int i = 0; i < gas.size(); i++) {
        sum = sum + (gas[i] - cost[i]);
        fuel = fuel + (gas[i] - cost[i]);
        if (fuel < 0) {
            fuel = 0;
            start_i = i + 1;
        }
    }

    if (sum >= 0) {
        return start_i % (gas.size());
    }

    return -1;
}
```
## Asked in

* Bloomberg
* Google
* DE
* Shaw
* Amazon
