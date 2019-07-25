# Transform CSV

https://www.interviewbit.com/problems/transform-csv/

Given a csv file(with , as a delimiter) named input with the following fields:

1. FirstName
2. LastName
3. Address
4. City
5. CountryCode
6. Email
7. PhoneNumber

Write a bash script to combine both CountryCode and PhoneNumber with a - and add a + before country code and remove country codes from the csv file

Example:

Assume that input has the following content:

```
Lotty,Kilner,08 Boyd Place,Jiangqiao,04,lkilner0@epa.gov,433-447-7966
Benoite,Ducket,9 Harper Alley,Tenenkou,22,bducket1@friendfeed.com,724-995-7769
```

Your script should output the following:

```
Lotty,Kilner,08 Boyd Place,Jiangqiao,lkilner0@epa.gov,+04-433-447-7966
Benoite,Ducket,9 Harper Alley,Tenenkou,bducket1@friendfeed.com,+22-724-995-7769
```

### Note
Note that the given csv file does not contain headers i.e., only data is present.

## Hint 1

Find out how to read a csv file in bash into different variable. Here the columns in the csv file is fixed.

## Solution Approach

Now that you have each column in a varible, simply make some transformations and overwrite the exisiting row.

## Solution
```bash
#!/bin/bash
while IFS=',' read -r f1 f2 f3 f4 f5 f6 f7
do
    echo "$f1,$f2,$f3,$f4,$f6,+$f5-$f7"
done < input
```

### Python
```python
def main():
    fp = open('input')
    while True:
        s = fp.readline()
        if not s:
            break
        fn,ln,ad,ct,cc,em,pn = s.split(',')
        print(','.join([fn,ln,ad,ct,em,'+'+cc+'-'+pn]),end='')
    return 0
if __name__ == '__main__':
    main()
```

