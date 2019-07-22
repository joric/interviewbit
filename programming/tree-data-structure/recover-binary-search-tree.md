# Recover Binary Search Tree

https://www.interviewbit.com/problems/recover-binary-search-tree/

Two elements of a binary search tree (BST) are swapped by mistake.
Tell us the 2 values swapping which the tree will be restored.

 Note:
A solution using O(n) space is pretty straight forward. Could you devise a constant space solution? 
Example :

```
Input : 
         1
        / \
       2   3

Output : 
       [1, 2]

Explanation : Swapping 1 and 2 will change the BST to be 
         2
        / \
       1   3
```
which is a valid BST

## Hint 1
In an ideal case, the inorder traversal should be sorted. Can you think how will the inorder traversal look like and how it can help you if you swap two element?

Tricky part now is devising a constant space solution for doing inorder traversal. There is a famous algorithm for same. Can you remember?

## Solution Approach

Lets look at the inorder traversal of the tree. 
In the ideal case, the inorder traversal should be sorted. But in this case, because of the swapping 2 cases might arise : 
1) A sequence like {1, 4, 3, 7, 9}, where the swapped pair are adjacent to each other. Only one inversion ( Inversion here means pair of integer A[i], A[i+1] where A[i] > A[i+1] ).
2) A sequence like {1, 9, 4, 5, 3, 10} where the swapped pair are not adjacent. 2 inversions. We take the min and max of the inversion numbers and we know the number we need to swap to get the right answer.

Now to figure this out, we need to do an inorder traversal. However, the traditional recursive inorder traversal has memory overhead of the depth of the tree.
Using a stack has the same memory overhead.

So, we need something which does not use recursion or stack and can still do the inorder traversal. This part is a bit tricky. Not all interviewers expect you to know this. 
Morris traversal helps us achieve what we are after. It works on the fact that we can modify the tree when traversing and then resetting the tree to its original state once we are done.

The idea of Morris traversal is based on Threaded Binary tree ( http://en.wikipedia.org/wiki/Threaded_binary_tree ).

In this traversal, we first create links to Inorder successor and print the data using these links, and finally revert the changes to restore original tree.

Initialize current as root
While current is not NULL
If current does not have left child
a) Print current's data
b) Go to the right, i.e., current = current->right
Else
a) Make current as right child of the rightmost node in current's left subtree
b) Go to this left child, i.e., current = current->left
Although the tree is modified through the traversal, it is reverted back to its original shape after the completion.




# Solution
```cpp
vector<int> Solution::recoverTree(TreeNode* A) {
    vector<int> sol;
    
    TreeNode *first = NULL, *last = NULL, *prev = NULL, *curr = A;
    
    stack<TreeNode*> st;
    bool isDone = false;
    
    while(!isDone){
        if (curr) {
            st.push(curr);
            curr = curr->left;
        } else {
            curr = st.top();
            st.pop();
            if(prev){
                if(prev->val > curr->val){
                    if(first == NULL){
                        first = prev;
                    }    
                    last = curr;
                }
            }
            prev = curr;
            curr = curr->right;
        }
        if(st.empty() &&  curr == NULL){
           isDone = true; 
        }
    }

    sol.push_back(first->val);
    sol.push_back(last->val);
    sort(sol.begin(), sol.end());
    return sol;
}
```

## Asked in

* Microsoft
* Amazon


