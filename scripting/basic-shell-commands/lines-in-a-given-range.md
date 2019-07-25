# Lines in a given range

https://www.interviewbit.com/problems/lines-in-a-given-range/

Write a bash script to print all the lines of the input which are in the given range.

The first line of the input contains two integers l and r separated by space.

You have to print all the lines of the file input which are in the range of [l, r].

Example:

Assume that the input has the following content.
```
10 15
Line 2
Line 3
Line 4
Line 5
Line 6
Line 7
Line 8
Line 9
Line 10
Line 11
Line 12
Line 13
Line 14
Line 15
Line 16
Line 17
Line 18
Line 19
Line 20
```
Your bash script should output the following
```
Line 10
Line 11
Line 12
Line 13
Line 14
Line 15
```
## Solution

## Hint 1
You could use:
```
sed
head
```

## Solution Approach

First Get the l and r

Use head to print r line and pipe it with tail to print last `r-l+1` lines

### Editorial
```bash
#!/bin/bash
l=$(cat input | head -n 1 | cut -d' ' -f1)
r=$(cat input | head -n 1 | cut -d' ' -f2)
cat input | head -n $r | tail -n $((r-l+1))
```

### Python
```python
def main():
    lines = open('input').read().splitlines()
    a, b = [int(x) for x in lines[0].split(' ')]
    for i in range(a, b+1):
        print(lines[i-1])

if __name__ == '__main__':
    main()
```
### Another Python
```python
def main():
    fp = open('input')
    line = fp.readline()
    a, b = [int(x) for x in line.split(' ')]
    for i in range(a-2):
        fp.readline()
    for i in range(b-a+1):
        print(fp.readline(), end='')
        
if __name__ == '__main__':
    main()

```
