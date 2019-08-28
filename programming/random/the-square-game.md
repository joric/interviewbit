# The square game

https://www.interviewbit.com/problems/the-square-game

You are given a square with sides parallel to the x and y axes.

You will be given the coordinates of the lower left and the 
upper right vertices of the square. You need to count the number of integral points that are strictly inside the square. 
The points that lie on the square should not be counted.

Image for sample case:

![](https://s3-us-west-2.amazonaws.com/ib-assessment-tests/problem_images/square.png)

### Constraints
```
1 <= A, B, C, D <= 1000
A != C
```

### Input
```
An integer A corresponding to the x coordinate of the lower left vertex.
An integer B corresponding to the y coordinate of the lower left vertex. 
An integer C corresponding to the x coordinate of the upper right vertex.
An integer D corresponding to the y coordinate of the upper right vertex.
```
### Output

One integer corresponding to the number of integral points strictly inside the given square.

### Examples
```
Input:

1
1
4
4
Output:

4
```

### Explanation

(2,3),(3,2),(2,2),(3,3) are the integral points that are strictly inside the given square. Refer the diagram given in the given question for more details.

## Hint 1
You have y2-y1-1 valid Y coordinates that can be choosen. Similary you have x2-x1-1 valid X coordinates that can be chosen.

## Solution Approach
You have y2-y1-1 valid Y coordinates that can be choosen. Similary you have x2-x1-1 valid X coordinates that can be chosen. 
So the total number of points strictly inside the square is (y2-y1-1)*(x2-x1-1)

## Solution
### Editorial
```cpp
int Solution::solve(int A, int B, int C, int D) {
    return (C-A-1)*(D-B-1);
}
```
### Fastest
```cpp
int Solution::solve(int A, int B, int C, int D) {
    return (abs(C-A-1)*abs(D-B-1));
}
```

### Lightweight
```cpp
int Solution::solve(int A, int B, int C, int D) {
    // return (D-B)*(C-A)-2*(C-A+1)-2*(D-B-1);
    return (D-B-1)*(C-A-1);
}
```

### Mine
```cpp
int Solution::solve(int A, int B, int C, int D) {
    return (C-A-1) * (D-B-1); // 200 points for that??
}
```
