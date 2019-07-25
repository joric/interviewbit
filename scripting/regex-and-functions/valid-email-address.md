# Valid Email Address

https://www.interviewbit.com/problems/valid-email-address/

Write a bash script that finds all the invalid email addresses.

For simplicity, assume that a vaild email addresses has the following rules-

Email should be of the form local@domain.com

There can only be alphanumberic characters in the local part email address.

```
The following characters are valid in the local part of the email as long as they are not the first character.

-, _, +, .

Email address can not start with a number.
Domain name can only contain alphanumeric characters and -.
com part can have atmost one ., for e.g. co.uk or co.in is valid but as.df.gh is invalid
```

### Example

Assume that input has the following content:
```
abc@example.co.uk
abc@example.com
abc<>@example.com
abc@example@gmail.com
```
Your script should output the following:
```
abc<>@example.com
abc@example@gmail.com
```

## Hint 1

Use Regular Expressions

## Solution Approach

Find the regex of valid email addresses and use grep -v to print invalid email addresses

## Solution

### Editorial
```bash
cat input | grep -xv "^[A-Za-z][-_\.\+A-Za-z0-9]*[@][-A-Za-z0-9]*[\.][A-Za-z]*" | grep -xv "^[A-Za-z][-_\.\+A-Za-z0-9]*[@][-A-Za-z0-9]*[\.][A-Za-z]*[\.][A-Za-z]*"
```

### Mine
```bash
#!/bin/bash
cat input | grep -vP '^[[:alpha:]][[:alnum:]-_+.]+@[[:alnum:]-]+\.[[:alnum:]-]*(\.)?[[:alnum:]-]+$'
```

