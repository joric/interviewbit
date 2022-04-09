# Triplets with Sum Between Given Range

https://www.interviewbit.com/problems/triplets-with-sum-between-given-range/

Given an array of real numbers greater than zero in form of strings.

Find if there exists a triplet (a,b,c) such that 1 < a+b+c < 2.

Return 1 for true or 0 for false.

Example:
```
Given [0.6, 0.7, 0.8, 1.2, 0.4] ,

You should return 1

as

0.6+0.7+0.4=1.7

1<1.7<2

Hence, the output is 1.
```

O(n) solution is expected.

Note: You can assume the numbers in strings don't overflow the primitive data type and there are no leading zeroes in numbers. Extra memory usage is allowed.

## Hint 1

The brute force approach would be using three loops and checking each triplet for the sum.It will be O(n^3).

Remember, we don't have to print the triplet. We just have to say whether it exists or not. 

Can you think of something better? 

Try bucketing the array.

## Hint 2

Start tackling this problem by thinking of kinds of numbers that could be members of potential solutions. Think about what range those numbers could have. From this, these are the few scenarios under which a valid triplet could exist.

We have natural boundaries at 0, 1, and 2. So that leads to a few scenarios. Suppose we let A=(0,1] and we let B=(1,2). Then, any number in a potential solution must come from either range A or range B.

That leaves us with four unique combinations:

```
1.A, A, A
2.A, A, B
3.A, B, B
4.B, B, B
```

We can quickly deduce that case 3 and case 4 are not possible. The minimum value for range B is a little bit more than 1. If we have two numbers that are a little bit more than 1, then our total sum will be a little bit more than 2. Say the numbers are 0.4, 1.0001 and 1.0001. Here sum is greater than 2. Hence, these cases won’t give us the required solution. Thus we can eliminate cases 3,4 and 5 (as they contain at least 2 numbers from range B).

So then we only have to check cases 1 and 2. Unfortunately, checking these cases is a little difficult. How can we determine if there are three numbers less than or equal to 1 that add up to a value greater than 1 and less than 2?

Maybe we can tighten the restrictions on case 1 to make it easier to solve. What if we knew that every number in range A was less than 2/3? Then we could just select the three highest values in A. If those three numbers exist and add up to a value in the range (1,2), then case 1 is fulfilled. Example- Let highest numbers in A be 0.333,0.55,0.44. These three numbers lie in the required range(1,2) and hence would give us the solution.

Now, since we tightened the restriction in case 1, we need another case to cover when values are in the range [2/3,1].

Let us formally define our new ranges. Let A=(0,2/3), B=[2/3,1] and C=(1,2).

These new ranges leave us with ten unique combinations:

```
1.A, A, A
2.A, A, B
3.A, A, C
4.A, B, B
5.A, B, C
6.A, C, C
7.B, B, B
8.B, B, C
9.B, C, C
10.C, C, C
```

Can you now solve the problem??


## Solution Approach

Start tackling this problem by thinking of kinds of numbers that could be members of potential solutions. Think about what range those numbers could have. From this, these are the few scenarios under which a valid triplet could exist.

We have natural boundaries at 0, 1, and 2. So that leads to a few scenarios. Suppose we let A=(0,1] and we let B=(1,2). Then, any number in a potential solution must come from either range A or range B.

That leaves us with four unique combinations:

```
1.A, A, A
2.A, A, B
3.A, B, B
4.B, B, B
```

We can quickly deduce that case 3 and case 4 are not possible. The minimum value for range B is a little bit more than 1. If we have two numbers that are a little bit more than 1, then our total sum will be a little bit more than 2. Say the numbers are 0.4, 1.0001 and 1.0001. Here sum is greater than 2. Hence, these cases won’t give us the required solution. Thus we can eliminate cases 3,4 and 5 (as they contain at least 2 numbers from range B).

So then we only have to check cases 1 and 2. Unfortunately, checking these cases is a little difficult. How can we determine if there are three numbers less than or equal to 1 that add up to a value greater than 1 and less than 2?

Maybe we can tighten the restrictions on case 1 to make it easier to solve. What if we knew that every number in range A was less than 2/3? Then we could just select the three highest values in A. If those three numbers exist and add up to a value in the range (1,2), then case 1 is fulfilled. Example- Let highest numbers in A be 0.333,0.55,0.44. These three numbers lies in the required range(1,2) and hence would give us the solution.

Now, since we tightened the restriction in case 1, we need another case to cover when values are in the range [2/3,1].

Let us formally define our new ranges. Let A=(0,2/3), B=[2/3,1] and C=(1,2).
These new ranges leave us with ten unique combinations:

```
1.A, A, A
2.A, A, B
3.A, A, C
4.A, B, B
5.A, B, C
6.A, C, C
7.B, B, B
8.B, B, C
9.B, C, C
10.C, C, C
```

We can quickly deduce that cases 6, 7, 8, 9, and 10 are not possible (the total sum will always be greater than 2).

That leaves us with cases 1, 2, 3, 4, and 5.

We can check case 1 by looking at the three largest values in A. Say we have these highest values as : 0.500,0.6666,0.65777. This lies between 1 and 2. We only have to worry about underflow in this case, meaning the sum of highest values in this range may not be greater than 1. But we are sure that this will be less than 2. So we only have to check for the condition whether these are greater than 1 or not.

Now, what about case 2? Under case 2, we have two numbers in range A and one number in range B. We have to worry about underflow and overflow. To avoid underflow, let’s suppose that we select the two largest values in A. Let’s call the sum of those numbers s. The range of s will be (0,4/3). So we just need to determine if there is a value in B that is greater than 1−s and less than 2−s. Simple enough.

Under case 3, we have two numbers in range A and one number in range C. We just have to worry about overflow ( because to the presence of an integer from range C, we are sure that their sum will be greater than 1). To avoid overflow, let’s suppose that we select the two smallest values in A and the smallest value in C. If the sum of those numbers is in the range ((1,2), then this case has occurred.

Case 4 will be similar to case 2. Under case 4, we have one number in range A and two numbers in range B. We have to worry about overflow. To avoid overflow, let’s suppose that we select the two smallest values in BB. Let’s call the sum of those numbers s. The range of ss will be [4/3,2]. So we just need to determine if there is a value in A that is less than 2−s. Not bad.

Case 5 is pretty easy as well. We have to worry about overflow. To avoid overflow, let’s suppose that we select the smallest value in A, the smallest value in B, and the smallest value in C. If the sum of those numbers is in the range (1,2), then this case has occurred.

So, to solve the problem, we just check to see if case 1, 2, 3, 4, or 5 is satisfied. Each case can be checked in O(n) time.

We can conclude that for all the 5 possible cases, we just need 3 largest values in range A, 2 smallest in range B, 2 smallest in range A and the smallest in range C. There are a lot of methods, choose anyone to find them.

## Solution

### Editorial

```cpp
double min_element(vector<double> A) { // return minimum element
    double min = A[0];
    for (int i = 0; i < A.size(); i++) {
        if (A[i] < min) {
            min = A[i];
        }
    }
    return min;
}
int Solution::solve(vector<string> &a) {
    vector<double> A, B, C;

    for (int i = 0; i < a.size(); i++) {
        char b[20];
        for (int j = 0; j < a[i].length(); j++) {
            b[j] = a[i][j];
        }
        if (0.0 < atof(b) &&
            atof(b) < ((double)2.0 / (double)3.0)) // atof converts string to double
        {
            A.push_back(atof(b));

        } else if ((double)2.0 / (double)3.0 <= atof(b) && atof(b) <= 1.0) {
            B.push_back(atof(b));

        } else if (1.0 < atof(b) && atof(b) < 2.0) {
            C.push_back(atof(b));
        }
    }

    // 1
    int res = 0;

    if (A.size() >= 3) {
        priority_queue<double> q(
            A.begin(),
            A.end()); // priority queue used to get max 3 elements in O(logn) time
        double m = 0;
        for (int i = 0; i <= 2; i++) {
            m += q.top();
            q.pop();
        }

        if (m > 1.0) {
            res = 1;
            return res;
        }
    }
    // 2
    if (A.size() >= 2 && B.size() >= 1) {
        priority_queue<double> q1(
            A.begin(),
            A.end()); // priority queue used to get max 2 elements in O(logn) time

        double m1 = 0;
        for (int i = 0; i <= 1; i++) {
            m1 += q1.top();
            q1.pop();
        }

        for (int i = 0; i < B.size(); i++) {
            if (1 - m1 < B[i] && B[i] < 2 - m1) {
                res = 1;
                return res;
            }
        }
    }

    // 3
    if (A.size() >= 2 && C.size() >= 1) {
        priority_queue<double, std::vector<double>, std::greater<double>> q2(
            A.begin(),
            A.end()); // priority queue used to get min 2 elements in O(logn) time
        double m2 = 0;
        for (int i = 0; i <= 1; i++) {
            m2 += q2.top();
            q2.pop();
        }

        double min = min_element(C);

        if (m2 + min < 2.0) {
            res = 1;
            return res;
        }
    }

    // 4
    if (B.size() >= 2 && A.size() >= 1) {
        priority_queue<double, std::vector<double>, std::greater<double>> q3(
            B.begin(),
            B.end()); // priority queue used to get min 2 elements in O(logn) time

        double m3 = 0;
        for (int i = 0; i <= 1; i++) {
            m3 += q3.top();
            q3.pop();
        }

        for (int i = 0; i < A.size(); i++) {
            if (A[i] < 2 - m3) {
                res = 1;
                return res;
            }
        }
    }

    // 5
    if (A.size() >= 1 && B.size() >= 1 && C.size() >= 1) {
        int res3 = 0;
        double min1 = min_element(A);
        double min2 = min_element(B);
        double min3 = min_element(C);
        if (min1 + min2 + min3 < 2 && min1 + min2 + min3 > 1) {
            res = 1;
            return res;
        }
    }

    return res;
    // Time complexity =O(logn)+O(n)
    // hence,Time complexity=O(n)
}
```

### Python 3

```python
class Solution:
    # @param A : list of strings
    # @return an integer
    def solve(self, A):
        n = len(A)
        B = [float(i) for i in A]
        buckets = [[], [], []]
        for i in B:
            if i < 2.0/3:
                buckets[0].append(i)
            elif i < 1:
                buckets[1].append(i)
            else:
                buckets[2].append(i)
        
        def get(index):
            amx1, amx2, amx3 = -10, -10, -10
            ami1, ami2, ami3 = 3, 3, 3
            for i in buckets[index]:
                if i > amx1:
                    amx1, amx2, amx3 = i, amx1, amx2
                elif i > amx2:
                    amx2, amx3 = i, amx2
                elif i > amx3:
                    amx3 = i
            
                if i < ami1:
                    ami1, ami2, ami3 = i, ami1, ami2
                elif i < ami2:
                    ami2, ami3 = i, ami2
                elif i < ami3:
                    ami3 = i
            return [amx1, amx2, amx3, ami1, ami2, ami3]
        
        
        a = get(0)
        b = get(1)
        c = get(2)
        ls = []
        fc = a[0] + a[1] + a[2]
        ls.append(fc)
        fc = a[3] + a[4] + c[3]
        ls.append(fc)
        fc = a[3] + b[3] + b[4]
        ls.append(fc)
        fc = a[3] + b[3] + c[3]
        ls.append(fc)
        fc = b[0] + a[3] + a[4]
        ls.append(fc)
        if a[0] != a[3]:
            fc = b[0] + a[0] + a[3]
            ls.append(fc)
            fc = b[3] + a[0] + a[3]
            ls.append(fc)
        fc = b[3] + a[0] + a[1]
        ls.append(fc)
        for fc in ls:
            if fc > 1 and fc < 2:
                return 1
        return 0
```
