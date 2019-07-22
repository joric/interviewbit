# Fizzbuzz

https://www.interviewbit.com/problems/fizzbuzz


## Solution

```cpp
vector<string> Solution::fizzBuzz(int A) {
    vector<string> B;
    for (int i=1; i<=A; i++) {
        if (i%3==0 && i%5==0)
            B.push_back("FizzBuzz");
        else if (i%3==0)
            B.push_back("Fizz");
        else if (i%5==0)
            B.push_back("Buzz");
        else
            B.push_back(to_string(i));
    }
    return B;
}

/*
vector<string> Solution::fizzBuzz(int A) {
    vector<string> B;
    for (int i=1; i<=A; i++)
        B.push_back(i%3==0 && i%5==0
            ? "FizzBuzz" : i%3==0
            ? "Fizz" : i%5==0
            ? "Buzz" : to_string(i));
    return B;
}
*/

/*
vector<string> Solution::fizzBuzz(int A) {
    vector<string> B;
    for (int i=1; i<=A; i++) {
        string s;
        if (i%3==0) s+="Fizz";
        if (i%5==0) s+="Buzz";
        B.push_back(s.length() ? s : to_string(i));
    }
    return B;
}

*/
```