# Ratio of Boys and Girls

https://www.interviewbit.com/problems/ratio-of-boys-and-girls/

In a country where everyone wants a boy, each family continues having babies
till they have a boy. After some time, what is the proportion of boys to girls in the country?

(Assuming probability of having a boy or a girl is the same)

Round off your answer to 2 decimal places and output the answer as I.xx
where I is the integer part of your answer, and xx are the first.

## Solution Approach

Assume there are C number of couples so there would be C boys. The number of girls can be calculated by the following method.

Number of girls = 0*(Probability of 0 girls) + 1* (Probability of 1 girl) + 2* (Probability of 2 girls)  +  ...
                = 0 + C/4 + 2*C/8 + 3*C/16 + ... 

## Solution

Assume there are C number of couples so there would be C boys. The number of girls can be calculated by the following method.

```
Number of girls = 0*(Probability of 0 girls) + 1*(Probability of 1 girl) + 2*(Probability of 2 girls) + ...
Number of girls = 0*(C*1/2) + 1*(C*1/2*1/2) + 2*(C*1/2*1/2*1/2) + ...
Number of girls = 0 + C/4 + 2*C/8 + 3*C/16 + ...
Number of girls = C
```

(using mathematical formulas; it becomes apparent if you just sum up the first 4-5 terms)

The proportion of boys to girls is 1 : 1.
As such, the answer would be 1.00



