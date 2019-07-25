# Remove Punctuations

https://www.interviewbit.com/problems/remove-punctuations/

Write a bash script that removes all the punctuations in the given file named input

For this question, assume that all of the following symbols are punctuations:

```
! @ # $ % ^ & * ( ) _ - + = { } [ ] ; : ' " ` / > ? . , < ~ | \
```

Example:

Assume that input has the following content:
```
This's the sunny day.
It is the sunny day, we can go out.
```
Your script should output the following:
```
Thiss the sunny day
It is the sunny day we can go out
```

## Hint 1

Can you do this using tr

## Solution Approach

Escape every character with a backslash

(or)

use `[:punct:]` to delete all punctuations.


## Solution

### Editorial
```bash
cat input | tr -d '[:punct:]'

(or)

cat input | tr -d '\!\@\#\$\%\^\&\*\(\)\_\-\+\=\{\}\[\];\:\\"\`\)\/\>\?\.\,\<\~\|\\' | tr -d "'"

```

### Python
```python
def main():
    p = set('!@#$%^&*()_-+={}[];:\'"`/>?.,<~|\\')
    fp = open('input')
    while True:
        line = fp.readline()
        if not line:
            break
        for c in line:
            if c not in p:
                print(c, end='')

if __name__ == '__main__':
    main()
```