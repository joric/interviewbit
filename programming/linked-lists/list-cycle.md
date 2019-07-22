# List Cycle

https://www.interviewbit.com/problems/list-cycle


Given a linked list, return the node where the cycle begins. If there is no cycle, return null.

Try solving it using constant additional space.

Example :

Input: 

                  ______
                 |     |
                 \/    |
        1 -> 2 -> 3 -> 4

Return the node corresponding to node 3. 

Approach

Lets now look at the starting point. 
If we were using hashing, the first repetition we get is the starting point. Simple!

What happens with the 2 pointer approach ?

Method 1 :

If you detect a cycle, the meeting point is definitely a point within the cycle.

Can you determine the size of the cycle ? ( Easy ) Let the size be k.
F ix one pointer on the head, and another pointer to kth node from head.
Now move them simulataneously one step at a time. They will meet at the starting point of the cycle.

Method 2 :

This might be slightly more complicated. It involves a bit of maths and is not as intuitive as method 1. 
Suppose the first meet at step k,the distance between the start node of list and the start node of cycle
is s, and the distance between the start node of cycle and the first meeting node is m. 
Then 
2k = (s + m + n1r) 
2(s + m + n2r) = (s + m + n1r) 
s + m = nr where n is an integer.
s = nr - m
s = (r - m) + (n-1)r

So, if we have one pointer on the head and another pointer at the meeting point.
Note that since the distance between start node of cycle and the first meeting node is m,
therefore if the pointer moves (r - m) steps, it will reach the start of the cycle.
When the pointer at the head moves s steps, the second pointer moves (r-m) + (n-1)r steps
which both points to the start of the cycle. In other words, both pointers meet first at the start of the cycle.


## Solution

```cpp

// editorial

ListNode *Solution::detectCycle(ListNode *head) {
    if (head == NULL || head->next == NULL)
        return NULL;

    ListNode *firstp = head;
    ListNode *secondp = head;
    bool isCycle = false;

    while (firstp != NULL && secondp != NULL) {
        firstp = firstp->next;
        if (secondp->next == NULL)
            return NULL;
        secondp = secondp->next->next;
        if (firstp == secondp) {
            isCycle = true;
            break;
        }
    }

    if (!isCycle)
        return NULL;

    firstp = head;

    while (firstp != secondp) {
        firstp = firstp->next;
        secondp = secondp->next;
    }

    return firstp;
}

// lightweight

ListNode *Solution::detectCycle(ListNode *A) {
    if (A == NULL)
        return A;

    ListNode *slow = A;
    ListNode *fast = A->next;

    while (fast && fast->next) {
        if (slow == fast) {
            slow = A;

            // detect node
            while (slow != fast->next) {
                slow = slow->next;
                fast = fast->next;
            }
            return fast->next;
        }

        slow = slow->next;
        fast = fast->next->next;
    }

    return NULL;
}

// rajalprogram

ListNode *Solution::detectCycle(ListNode *head) {
    if (!head || !head->next)
        return 0;
    ListNode *slow = head, *fast = head;
    while (fast) {
        if (!fast->next || !fast->next->next)
            return 0;
        slow = slow->next;
        fast = fast->next->next;
        if (slow == fast)
            break;
    }
    slow = head;
    while (slow != fast) {
        slow = slow->next;
        fast = fast->next;
    }

    return slow;
}

ListNode *Solution::detectCycle(ListNode *head) {
    if (!head || !head->next)
        return 0;

    ListNode *slow = head, *fast = head;

    while (slow && fast) {
        slow = slow->next;
        if (!fast->next)
            return 0 else fast = (fast->next)->next;
        if (slow == fast)
            break;
    }

    if (!slow || !fast)
        return 0

                   ListNode *
                   curr = head;

    while (curr != slow) {
        curr = curr->next;
        slow = slow->next;
    }

    return curr;
}
```