# Copy List

https://www.interviewbit.com/problems/copy-list



## Hint 1

Easiest way to approach this problem is using hashmap. How? Try to think how you can maintain
link between old nodes and new nodes.

Another efficient way can be creating new nodes and updating the random connections by traversing list again.

Solution approach

There are 2 approaches to solving this problem.

Approach 1: Using hashmap.
Use a hashmap to store the mapping from oldListNode to the newListNode.
Whenever you encounter a new node in the oldListNode (either via random pointer or through the next pointer ),
create the newListNode, store the mapping. and update the next and random pointers of the newListNode
using the mapping from the hashmap.

Approach 2: Using 2 traversals of the list. 
Step 1: create a new node for each existing node and join them together eg: A->B->C will be A->A'->B->B'->C->C'

Step2: copy the random links: for each new node n', n'.random = n.random.next

Step3: detach the list: basically n.next = n.next.next; n'.next = n'.next.next


## Solution

```cpp


RandomListNode* Solution::copyRandomList(RandomListNode* A) {
    
    if(A == NULL){
        return NULL;
    }
    
    RandomListNode* head = A;
    RandomListNode* cloned;
    RandomListNode* newHead = new RandomListNode(head->label);
    RandomListNode* curr = head;
    RandomListNode* temp = newHead;
    
    unordered_map<RandomListNode*, RandomListNode*> myMap;
    
    myMap.insert({head, newHead});
    curr = curr->next;
    while(curr != NULL){
        cloned = new RandomListNode(curr->label);
        temp->next = cloned;
        temp = temp->next;
        myMap.insert({curr, cloned});
        curr = curr->next;
    }
    
    curr = A;
    temp = newHead;
    while(curr != NULL){
        if((myMap.find(curr->random)) != myMap.end()){
            myMap[curr]->random = myMap[curr->random];    
        }
        else{
            myMap[curr]->random = NULL;
        }
        curr = curr->next;
    }
    
    return newHead;
}


```