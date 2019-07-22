# Populate Next Right Pointers Tree

https://www.interviewbit.com/problems/populate-next-right-pointers-tree/

Given a binary tree
```
    struct TreeLinkNode {
      TreeLinkNode *left;
      TreeLinkNode *right;
      TreeLinkNode *next;
    }
```

Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to NULL.

Initially, all next pointers are set to NULL.

Note:
You may only use constant extra space.

Example :

Given the following binary tree,
```
         1
       /  \
      2    3
     / \  / \
    4  5  6  7
```
After calling your function, the tree should look like:
```
         1 -> NULL
       /  \
      2 -> 3 -> NULL
     / \  / \
    4->5->6->7 -> NULL
```
Note 1: that using recursion has memory overhead and does not qualify for constant space.
Note 2: The tree need not be a perfect binary tree. 

## Hint 1

Have you tried making use of the next links you are creating ?

## Solution approach

Lets say you have already created next pointers till level L. To create next pointer for level L+1, start with the first node on level L.
```
        1   ->  2   ->  3  -> 4
       /         \           / \
      /           \         /    \ 
     5            6        7      8 
        

        1   ->  2   ->  3  -> 4
       /         \           / \
      /           \         /   \ 
     4     ->      7   ->  8 ->  9 
```
Keep track of the previous node you encountered on the next level. For the current node, explore the left and right child if they are not null in that order. If the prev is not set, that means we are looking at the leftmost node on the next level ( Lets store it because we will begin the next iteration from this node ). Else you can assign prev->next as the current node in next level you are exploring and update the prev node.


## Solution

### Fastest

```cpp
void Solution::connect(TreeLinkNode* root) {
     while(root) {
         TreeLinkNode* t=new TreeLinkNode(0);
         TreeLinkNode* c=t;
         while(root) {
             if(root->left) {
                 c->next=root->left;
                 c=c->next;
             }
             if(root->right) {
                 c->next=root->right;
                 c=c->next;
             }
             root=root->next;
         }
         root=t->next;
     }
}
```

### Editorial

```cpp
class Solution {
public:
    void connect(TreeLinkNode *root) {
        TreeLinkNode* leftWall = NULL; // leftmost node of the next level.
        TreeLinkNode* prev = NULL; // leading node on the next level
        TreeLinkNode* cur = root; // current node on the current level

        while (cur != NULL) {
            
            while (cur != NULL) {
                if (cur->left != NULL) {
                    if (prev != NULL) {
                        prev->next = cur->left;
                    } else {
                        leftWall = cur->left;
                    }
                    prev = cur->left;
                }

                if (cur->right != NULL) {
                    if (prev != NULL) {
                        prev->next = cur->right;
                    } else {
                        leftWall = cur->right;
                    }
                    prev = cur->right;
                }

                // move to the next node
                cur = cur->next;
            }

            // move to the next level
            cur = leftWall;
            leftWall = NULL;
            prev = NULL;

        }
    }
};
```

### My


```cpp
void Solution::connect(TreeLinkNode* A) {
    if(A == NULL)return;
    
    queue<pair<int, TreeLinkNode*> > q;
    
    q.push({0, A});

    while(!q.empty()){
        pair<int, TreeLinkNode*> temp = q.front();

        int level = temp.first;
        q.pop();

//      Keep putting the nodes until we are on the same level.
        while(!q.empty() and q.front().first == level){
            if(temp.second->left)
                q.push({level+1, temp.second->left});
            if(temp.second->right)
                q.push({level+1, temp.second->right});

            temp.second->next = q.front().second;
            temp = q.front();

            q.pop();
        }
//      
        temp.second->next = NULL;
        
//      if the control doesn't enter the above while loop, accomodate
//      the left out nodes. (Consider a three node tree!)
        if(temp.second->left)
            q.push({level+1, temp.second->left});
        if(temp.second->right)
            q.push({level+1, temp.second->right});
        
    }
}
```

## Asked in

* Microsoft
* Amazon

