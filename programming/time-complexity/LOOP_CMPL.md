# LOOP_CMPL

* https://www.interviewbit.com/problems/loopcmpl

What is the time, space complexity of following code:

```
int a = 0, b = 0;
for (i = 0; i < N; i++) {
    a = a + rand();
}
for (j = 0; j < M; j++) {
    b = b + rand();
}
```

Assume that rand() is O(1) time, O(1) space function.

## Solution

```
O(N + M) time, O(1) space
```
