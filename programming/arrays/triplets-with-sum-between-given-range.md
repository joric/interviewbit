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

The brute force approach would be using three loops and checking each triplet for the sum.It will be O(n³).

Remember, we don’t have to print the triplet. We just have to say whether it exists or not. 

Can you think of something better? 

Try bucketing the array.

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

## Solution

### Editorial
```cpp
int Solution::solve(vector<string> &arr) {
    int n = arr.size(), i;
    vector<float> v;
    for (i = 0; i < n; i++) {
        v.push_back(stof(arr[i]));
    }
    float a = v[0], b = v[1], c = v[2];

    float mx = 0;
    for (i = 3; i < n; i++) {
        if (a + b + c < 2 && a + b + c > 1)
            return 1;
        else if (a + b + c > 2) {
            if (a > b && a > c)
                a = v[i];
            else if (b > a && b > c)
                b = v[i];
            else
                c = v[i];
        } else {
            if (a < b && a < c)
                a = v[i];
            else if (b < a && b < c)
                b = v[i];
            else
                c = v[i];
        }
    }
    if (a + b + c > 1 && a + b + c < 2)
        return 1;
    else
        return 0;
}

```

### Another Solution

```cpp
int Solution::solve (vector < string > &A) {
    vector < double > arr;
    
    for (auto s:A)
        arr.push_back (stod (s));

    double a = arr[0], b = arr[1], c = arr[2];
    
    for (int i = 3; i < A.size ()+1; i++) {
        double sum = a+b+c;
        
        if (sum>1 && sum<2)
            return 1;
        
        if (i>=A.size())
            break;
        
        double x = arr[i];
        double m = sum>2 ? max(a, max(b,c)) : sum<=1 ? min(a, min(b,c)) : -1;
        
        if (m==a) a=x;
        else if (m==b) b=x;
        else if (m==c) c=x;
    }
    return 0;
}
```
### Another Solution
```cpp
int Solution::solve (vector < string > &A) {
    vector < double > arr;
    
    for (auto s:A)
        arr.push_back (stod (s));

    double a = arr[0], b = arr[1], c = arr[2];
    
    for (int i = 3; i < A.size ()+1; i++) {
        double sum = a+b+c;
        
        if (sum>1 && sum<2)
            return 1;
        
        if (i>=A.size())
            break;
        
        double x = arr[i];
        
        if (sum>2) {
            double m = max(a, max(b,c));
            if (m==a) a=x; else if (m==b) b=x; else if (m==c) c=x;
        } else if (sum<=1) {
            double m = min(a, min(b,c));
            if (m==a) a=x; else if (m==b) b=x; else if (m==c) c=x;
        }
    }
    return 0;
}
```
### Another Solution
```cpp
int Solution::solve (vector < string > &A) {
	vector < double >arr;
for (auto s:A)
		arr.push_back (stod (s));

	double a = arr[0], b = arr[1], c = arr[2];
	for (int i = 3; i < A.size (); i++) {
		// check if sum fall in (1, 2)
		if (a + b + c > 1 && a + b + c < 2) {
			return 1;
		}
		// if not, then check is sum greater than 2
		// if so, then replece MAX(a,b,c) to new number
		else if (a + b + c > 2) {
			if (a > b && a > c) {
				a = arr[i];
			}
			else if (b > a && b > c) {
				b = arr[i];
			}
			else if (c > a && c > b) {
				c = arr[i];
			}
		}
		// else then sum must be less than 1
		// then replace MIN(a,b,c) to new number
		else {
			if (a < b && a < c) {
				a = arr[i];
			}
			else if (b < a && b < c) {
				b = arr[i];
			}
			else if (c < a && c < b) {
				c = arr[i];
			}
		}
	}
	// check for last a, b, c  triplet
	if (a + b + c > 1 && a + b + c < 2) {
		return 1;
	}
	else {
		return 0;
	}

}
```
