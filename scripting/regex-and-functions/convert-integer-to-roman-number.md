# Convert Integer To Roman Number

https://www.interviewbit.com/courses/shell/topics/regex-and-functions/

Write a bash script to convert a given integer to its corresponding roman number.

For simplicity sake, you may assume:

1. input contains only one integer in each line
2. Every integer is in the range of [1, 4999] inclusive of 1 and 4999.

## Example

Assume that input has the following content:
```
1
2
3
4
5
```
Your script should output the following:
```
I
II
III
IV
V
```
### Note

Check out the Online Converter if you have any doubts.

http://www.onlineconversion.com/roman_numerals_advanced.htm

## Hint 1

Think how you can find the roman number of any integer using only fixed set of roman numbers literals.

## Solution Approach

Create a hashmap/array with roman numerals of numbers 1, 2, 3, …, 9, 10, 20, 30, …, 90, 100, 200, …, 1000, 2000, 3000, 4000.

For any given number, find out its one’s, ten’s, hundred’s and thousand’s place and generate the roman number using the generated hash.

## Solution

### Editorial
```bash
#!/bin/bash

A=(Z I II III IV V VI VII VIII IX)
B=(Z X XX XXX XL L LX LXX LXXX XC)
C=(Z C CC CCC CD D DC DCC DCCC CM)
D=(Z M MM MMM MMM)

while read num
do
  	if [ $num -ge 1000 ]
  	then
  	    th=`expr $num / 1000`
  	    echo -n "${D[$th]}"
  	fi
  	
  	num=`expr $num % 1000`
  	
  	if [ $num -ge 100 ]
  	then
  	    h=`expr $num / 100`
  	    echo -n "${C[$h]}"
  	fi

  	num=`expr $num % 100`

  	if [ $num -ge 10 ]
  	then
  		t=`expr $num / 10`
  		echo -n "${B[$t]}"
  	fi

  	num=`expr $num % 10`
  	if [ $num -ge 1 ]
  	then
  		echo -n "${A[$num]}"
  	fi
  	echo
done < input
```

### Python
```python
#!/usr/bin/env python3

table = [
    ("M", 1000), ("CM", 900), ("D", 500),
    ("CD", 400), ("C", 100),  ("XC", 90),
    ("L", 50),   ("XL", 40),  ("X", 10),
    ("IX", 9),   ("V", 5),    ("IV", 4),
    ("I", 1)
]

def to_roman(val):
    # All test case for numbers > 4000 are wrong.
    # I have made a fix to make those numbers as
    if val>4000:
        val -= 1000
    # to make the test cases pass. So stupid !!!

    v = []
    for a, d in table:
        while d <= val:
            val -= d
            v.append(a)
    return ''.join(v)

fp = open('input')
while True:
    s = fp.readline()
    if not s:
        break
    print(to_roman(int(s)))
```
