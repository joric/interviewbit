# Grep

https://www.interviewbit.com/problems/grep/

The following command prints all the lines of the file input which contains a number

`cat input | grep '[0-9]*'`

Example:
Assume that the input has the following content:
```
axes12
w143th
qwer
```
Then the given command prints the following ouput:
```
axes12
w143th
```

Now change the command slighly so that it prints only the number part of the lines.


Example:
Assume that the input has the following content:
```
axes12
w143th
qwer
```
Then your new command should ouput the following content
```
12
143
```

## Hint 1
Read about different flags of grep.

## Solution Approach
How do you print only the matched pattern using grep?

## Solution
```bash
cat input | grep -o '[0-9]*'
```

