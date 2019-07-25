# Sort by Frequency

https://www.interviewbit.com/problems/sort-by-frequency/

In the given file named input, find the frequency of all the words and print as per the following format.

The first column of each line of the output should be the frequency of the word followed by all the words of that frequency arranged in lexicographical order separated with space “ ”

Sort the words in the ascending order of frequency.

For simplicity, assume that

Words are case sensitive, i.e. The and the are treated as different words.

Example:

Assume that input has following content:
```
the day is sunny 
it is the sunny day
we can go out
```
Your script should output the following, sorted by the ascending frequency:

```
1 can go it out we
2 day is sunny the
```
## Hint 1
Can you use tr command to convert the given sentences into words.

## Solution Approach

breakdown the problem into subparts and try to solve each subpart with a single command and pipe them in the end.

Subproblems are

1. convert the file in such way that each line has a single word
2. sort the words in lexicographical order
3. group words and note their count
4. then sort based on the count of each word
5. then group by count
6. sort each line lexicographically

## Solution

### Editorial
```bash
cat input | tr -s ' ' '\n' | sort | uniq -c | sort | tr -s '[:space:]' | awk -F' ' '$1==last {printf " %s",$2; next} NR>1 {print "";} {last=$1; printf "%s",$0;} END{print "";}' | sed "s/^[ \t]*//"
```

### Mine
```bash
cat input | tr ' ' '\n' | sort | uniq -c | awk '{ a[$1] = a[$1]=="" ? $2 : a[$1]" "$2 } END { for (key in a) { print key" "a[key] } }' | sort -n
```

### Python
```python
def main():
    f, d = {},{}
    for w in open('input').read().split():
        f[w] = f.get(w,0)+1
    for k,v in f.items():
        d[v] = d.get(v,[])+[k]
    for k,v in sorted(d.items()):
        print(k, ' '.join(sorted(v)))
if __name__ == '__main__':
    main()
```


