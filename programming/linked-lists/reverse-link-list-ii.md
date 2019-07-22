# Reverse Linked List II

https://www.interviewbit.com/problems/reverse-linked-list-ii



## Hint 1

Lets first look at the problem of reversing the linked list.

a -> b -> c should become
a <- b <- c

Obviously at every instant, you need to know about the previous of the node,
so that you can link it to the next of the node. 
Can you think along the lines of maintaining a previousNode, curNode and nextNode ?

Now once you know how to reverse a linked List, how can you apply it to the current problem ?

## Solution approach

If you are still stuck at reversing the full linked list problem, 
then would maintaining curNode, nextNode and a tmpNode help ?

Maybe at every step, you do something like this :

    tmp = nextNode->next;
            nextNode->next = cur;
            cur = nextNode;
            nextNode = tmp;

Now, lets say you did solve the problem of reversing the linked list and are stuck at applying it to current problem. 
What if your function reverses the linked list and returns the endNode of the list. 
You can simply do 
prevNodeOfFirstNode->next = everseLinkedList(curNode, n - m + 1);

## Solution

```cpp

// lightweight

ListNode *Solution::reverseBetween(ListNode *A, int m, int n) {

    ListNode *next, *current, *prev, *start, *sprev;
    current = A;
    prev = NULL;
    sprev = NULL;

    int c = 1;
    while (c < m) {
        sprev = current;
        current = current->next;
        start = current;
        c++;
    }

    c++;

    prev = current;
    start = current;
    current = current->next;
    while (c <= n) {
        next = current->next;
        current->next = prev;
        prev = current;
        current = next;
        c++;
    }

    if (sprev == NULL) {
        A = prev;
    } else {
        sprev->next = prev;
    }

    start->next = current;

    return A;
}

/// something

ListNode *Solution::reverseBetween(ListNode *A, int m, int n) {
    if (!A)
        return NULL;
    if (m == n)
        return A;
    ListNode *temp, *current, *prev, *next, *start, *end;
    int i = 1;
    temp = A;
    bool flag = false;
    if (m == 1)
        flag = true;
    while (i < m) {
        start = temp; //(m-1)th node
        temp = temp->next;
        ++i;
    }
    current = next = temp;

    while (i < n) {
        temp = temp->next;
        ++i;
    }
    end = temp; //nth node
    prev = temp->next;
    ListNode *endplus = end->next;
    while (current != endplus) {
        next = current->next;
        current->next = prev;
        prev = current;
        current = next;
    }
    if (flag)
        A = prev;
    else
        start->next = prev;
    return A;
}
```