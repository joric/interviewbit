# Sorted Permutation Rank With Repeats

https://www.interviewbit.com/problems/sorted-permutation-rank-with-repeats



If you have not solved RANK problem, we advice you to take a look into that. 
If you have, lets move forward here.

Enumerating all permutations and matching with the current one is going to be exponential.

Lets start by looking at the first character.

If the first character is X, all permutations which had the first character less than X would come before this permutation when sorted lexicographically.

Number of permutation with a character C as the first character = number of permutation possible with remaining N-1 character = (N-1)! / (p1! * p2! * p3! ... ) where p1, p2, p3 are the number of occurrences of repeated characters.

For example, number of permutations possible with 3 'a' and 3 'b' is 6! / 3! 3! = 20

Can you use the above information to get the rank of the current permutation ?



hint 2

Lets start by looking at the first character.

If the first character is X, all permutations which had the first character less than X would come before this permutation when sorted lexicographically.

Number of permutation with a character C as the first character = number of permutation possible with remaining N-1 character = (N-1)! / (p1! * p2! * p3! ... ) where p1, p2, p3 are the number of occurrences of repeated characters.

For example, number of permutations possible with 3 'a' and 3 'b' is 6! / 3! 3! = 20

Hence,

rank = number of permutation possible with placing characters smaller than current first character in the first position + rank of permutation of string with the first character removed
So, we take a loop on the possibilities for the first character, and if that character is less than the current first character, we calculate the number of permutations using the formula given above (N-1)! / (p1! * p2! * p3! ... )

Now, there is a slight problem. 
(N-1)! / (p1! * p2! * p3! ... ) does not necessarily fit in an integer. It is easy to calculate (N-1)! % MOD. 
However, how do we handle divisions ? Modular_multiplicative_inverse is what you are looking for.
In short,

(1/A) % MOD = A ^ (MOD - 2) % MOD




## Solution

```cpp

/* my */

#define mod 1000003

int fact(int n) {
	return n ? (n * fact(n - 1)) % mod : 1;
}

int Solution::findRank(string s) {
	int ans = 0, n = s.length();
	for (int i = 0; i < n - 1; i++) {
		int c = 0;
		for (int j = i + 1; j < n; j++) {
			if (s[j] < s[i])
				c++;
		}
		ans += ((c * fact(n - i - 1))) % mod;
	}
	return (ans + 1) % mod;
}

/* editorial */

long long int pow_mod(long long int a, long long int b) {
	long long MOD = 1000003;
	if (a == 1)
		return 1;
	long long int x = 1, y = a;
	while (b > 0) {
		if (b % 2) {
			x = (x * y) % MOD;
		}
		y = (y * y) % MOD;
		b = b >> 1;
	}
	return x;
}

int Solution::findRank(string A) {
	long long ans = 0;
	long long mod = 1000003;
	long long arr[300];
	long long n = A.length();
	long long fact[n];
	fact[0] = 1;
	for (int i = 1; i < n; i++) {
		fact[i] = ((fact[i - 1] % mod) * (i % mod)) % mod;
	}
	for (long long i = 0; i < 300; i++)
		arr[i] = 0;

	for (long long i = 0; i < n; i++) {

		arr[A[i]]++;
	}
	// for(long long i=0;i<26;i++)
	//   cout<<arr[i]<<" ";

	for (long long i = 0; i < n; i++) {

		long long cnt = 0;
		long long di = 1;

		for (long long j = (A[i] - 1); j >= 0; j--) {
			cnt += arr[j];
		}

		// cout<<cnt<<" ";

		for (int j = 0; j < 300; j++) {

			di = (di % mod * fact[arr[j]] % mod) % mod;
		}
		long long a = pow_mod(di, (mod - 2)) % mod;

		// cout<<di<<" ";

		ans = (ans + ((cnt * fact[n - i - 1]) % mod * a) % mod) % mod;

		//  cout<<ans<<" ";
		arr[A[i]]--;
	}
	++ans;
	return ans % mod;
}
```