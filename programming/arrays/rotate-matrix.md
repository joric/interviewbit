# Rotate Matrix

https://www.interviewbit.com/problems/rotate-matrix/

You are given an n x n 2D matrix representing an image.

Rotate the image by 90 degrees (clockwise).

You need to do this in place.

Note that if you end up using an additional array, you will only receive partial score.

Example:

If the array is
```
[
    [1, 2],
    [3, 4]
]
```
Then the rotated array becomes:
```
[
    [3, 1],
    [4, 2]
]
```

## Hint 1

Lets say our matrix is ,

```
* * * ^ * * *
* * * | * * *
* * * | * * *
* * * | * * *
```

After rotating right, it appears (observe arrow direction)

```
* * * *
* * * *
* * * *
-- - - >
* * * *
* * * *
* * * *
```
The idea is simple. Transform each row of source matrix into required column of final matrix.

From the above picture, we can observe that

first row of source ------> last column of destination

second row of source ------> last but-one column of destination

so ... on

last row of source ------> first column of destination

Thus, if we were allowed extra memory, the solution should be easy.

    result[j][matrix.size() - i - 1] = matrix[i][j];

Now that you know the basic relation, can you do it in place ( without using extra memory ) ?

## Solution Approach

Doing things in place can be slightly trickier.

Note that if you create a graph with each cell as vertex with an edge from source cell to the destination cell, the graph ends up with cycles of length 4.

Which means something like this should work:

Divide the array into 4 along the diagonals

Place each element in the top quadrant into the slot 90 degrees clockwise

Place the old 90 in 180 degrees clockwise

Place the old 180 in 270 degrees

Lastly, place the old 270 in the original place

It might be easier to understand the solution if a few examples are tried out by hand. 

Let me elaborate on a 3x3 example. You can try more examples of other size. 

Lets say the array is

```
[ 
1 2 3
4 5 6
7 8 9
]
```

After rotation by 90 degree, it should be

```
[
7 4 1
8 5 2
9 6 3
]
```

Lets say you were allowed extra space and not required to do things in place.

Easier approach : 
If you notice, if you read out the column i in reverse order, it corresponds to the new row i in resulting array.
So, column 0 in original array read out in reverse order is 7 4 1 which is row 0 in answer.
And so on. So you just simulate this approach and keep creating the result in another 2 D array.

However, this approach requires additional space of O(n^2) where n = number of rows in 2D array.

Now lets say we have to do things in place ( no extra space allowed ). This implies that we have to make things work with just moving elements around with constant extra memory.
So, lets try to observe where elements need to end up in the result array.

* 7 needs to end up in 1's position. 
* If 7 goes to 1's position, then we need to check where 1 needs to go otherwise value 1 will be lost. 1 needs to go to 3's position. 
* 3 needs to go to 9's position. 
* 9 needs to go to 7's position. 
* We already have placed 7 in 1's position. 

So when we move these 4 elements, all 4 elements are in their correct position.

Lets look at 4 now.

4 -> 2 -> 6 -> 8. 

Again, a group of 4. So, we can move these elements group by group without requiring creating a copy of the array.

You can try a few more examples to convince yourself.

The code just tries to simulate whats being discussed here.

## Solution

### Editorial

```cpp
void Solution::rotate(vector<vector<int> > &matrix) {
    int len = matrix.size();
    for (int i = 0; i < len / 2; i++) {
        for (int j = i; j < len - i - 1; j++) {
            int tmp = matrix[i][j];
            matrix[i][j] = matrix[len - j - 1][i];
            matrix[len - j - 1][i] = matrix[len - i - 1][len - j - 1];
            matrix[len - i - 1][len - j - 1] = matrix[j][len - i - 1];
            matrix[j][len - i - 1] = tmp;
        }
    }
}
```

### Simple (programming pearls)

```cpp
void Solution::rotate(vector<vector<int> > &A) {
    int n = A.size();

    // transpose matrix
    for (int i = 0; i < n; i++) 
        for (int j = i; j < n; j++) 
            swap(A[i][j], A[j][i]);

    // reverse columns
    for (int i=0; i<n; i++)
        reverse(A[i].begin(), A[i].end());
}
```

## Asked in

* Google
* Facebook
* Amazon

