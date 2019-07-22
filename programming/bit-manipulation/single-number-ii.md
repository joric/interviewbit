# Single Number II

https://www.interviewbit.com/problems/single-number-ii



Given an array of integers, every element appears thrice except for one which occurs once.

Find that element which does not appear thrice.

Note: Your algorithm should have a linear runtime complexity.

Could you implement it without using extra memory?


http://javabypatel.blogspot.com/2015/09/find-number-that-appears-once.html

## Solution

```cpp

/* editorial */

int singleNumber(const vector<int> &A) {
        int n = A.size();
    int count[32] = {0};

    int result = 0;

    for (int i = 0; i < 32; i++) {
        for (int j = 0; j < n; j++) {
            if ((A[j] >> i) & 1) {
                count[i]++;
            }
        }
        result |= ((count[i] % 3) << i);
    }
    return result;
}

/* lightweight */

/*
ones - At any point of time, this variable holds XOR of all the elements which have
appeared "only" once.

twos - At any point of time, this variable holds XOR of all the elements which have
appeared "only" twice.

So if at any point time,

A new number appears - It gets XOR'd to the variable "ones".
A number gets repeated(appears twice) - It is removed from "ones" and XOR'd to the
variable "twice".
A number appears for the third time - It gets removed from both "ones" and "twice".

*/

int Solution::singleNumber(const vector<int> &A) {
    int ones = 0, twos = 0;
    for(int i = 0; i < A.size(); i++){
        ones = (ones ^ A[i]) & ~twos;
        twos = (twos ^ A[i]) & ~ones;
    }
    return ones;
}


/* comments */

int Solution::singleNumber(const vector<int> &A) {
    int first = 0;
    int second = 0;
    for(auto n:A){
        first = (first ^ n) & ~second;
        second = (second ^ n) & ~first;
    }
    return first;
}

/* shorter */
int Solution::singleNumber (const vector < int > &A) {

    int a = 0;
    int b = 0;

    for (int i = 0; i < A.size(); i++) {
        b |= a & A[i];
        a ^= A[i];
        int c = ~(a & b);
        a &= c;
        b &= c;
    }
    return a;
}

/* longer */

int Solution::singleNumber (const vector < int >&arr) {

	int firstTimeOccurence = 0;
	int secondTimeOccurence = 0;

	for (int i = 0; i < arr.size (); i++) {
		secondTimeOccurence = secondTimeOccurence | (firstTimeOccurence & arr[i]);
		firstTimeOccurence = firstTimeOccurence ^ arr[i];
		int neutraliser = ~(firstTimeOccurence & secondTimeOccurence);
		firstTimeOccurence = firstTimeOccurence & neutraliser;
		secondTimeOccurence = secondTimeOccurence & neutraliser;
	}
	return firstTimeOccurence;
}

```
