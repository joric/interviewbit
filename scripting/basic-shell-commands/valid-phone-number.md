# Valid phone number

https://www.interviewbit.com/problems/valid-phone-number/

Given a text file input that contains list of phone numbers (one per line).

Write a bash script to print all valid phone numbers.You may assume that a valid phone number must appear in one of the following two formats:

1. (xxx) xxx-xxxx
2. xxx-xxx-xxxx

You may also assume each line in the text file must not contain leading or trailing white spaces.

### Sample Input
```
987-123-4567
123 456 7890
(123) 456-7890
```
### Sample output
```
987-123-4567
(123) 456-7890
```

## Hint 1

you need to use grep

## Solution Approach

you need to use grep along with a regular expression.

## Solution
### Tutorial

```bash
grep -P '^(\d{3}-|\(\d{3}\) )\d{3}-\d{4}$' input

```

### Mine
```bash
cat input | tr -s ' ' | grep "^(*[0-9]\{3,3\})*[ -][0-9]\{3,3\}-[0-9]\{4,4\}$"
```
