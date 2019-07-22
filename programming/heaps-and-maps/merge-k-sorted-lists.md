# Merge K Sorted Lists

https://www.interviewbit.com/problems/merge-k-sorted-lists

Merge k sorted linked lists and return it as one sorted list.

### Example
```
1 -> 10 -> 20
4 -> 11 -> 13
3 -> 8 -> 9
will result in

1 -> 3 -> 4 -> 8 -> 9 -> 10 -> 11 -> 13 -> 20
```

## Hint 1

Lets keep a pointer for every linked list. At any instant you will need to know the minimum of elements at all pointer. Following it you will need to advance that pointer. Can you do this in complexity better than O(K).

Preferably O(logK). But how?

## Solution Approach

There are 2 approaches to solving the problem.

#### Approach 1: Using heap

At every instant, you need the minimum of the head of all the k linked list.
Once you know the minimum, you can push the node to your answer list, 
and move over to the next node in that linked list.

### Approach 2: Divide and conquer

Solve the problem for first k/2 and last k/2 list. Then you have 2 sorted lists. Then simiply merge the lists. 

Analyze the time complexity. 
```
T(N) = 2 T(N/2) + N 
T(N) = O (N log N)
```

## Solution

### Editorial
```cpp
struct CompareNode {
    bool operator()(ListNode *const &p1, ListNode *const &p2) {
        return p1->val > p2->val;
    }
};

ListNode *Solution::mergeKLists(vector<ListNode *> &lists) {
    ListNode *dummy = new ListNode(0);
    ListNode *tail = dummy;

    priority_queue<ListNode *, vector<ListNode *>, CompareNode> queue;

    for (int i = 0; i < lists.size(); i++) {
        if (lists[i] != NULL) {
            queue.push(lists[i]);
        }
    }

    while (!queue.empty()) {
        tail->next = queue.top();
        queue.pop();
        tail = tail->next;

        if (tail->next) {
            queue.push(tail->next);
        }
    }

    return dummy->next;
}
```

### Mine
```cpp
ListNode *Solution::mergeKLists(vector<ListNode *> &lists) {
    ListNode *dummy = new ListNode(0);
    ListNode *tail = dummy;

    auto comp = [](ListNode *a, ListNode *b) { return a->val > b->val; };

    priority_queue<ListNode *, vector<ListNode *>, decltype(comp)> queue(comp);

    for (int i = 0; i < lists.size(); i++)
        if (lists[i])
            queue.push(lists[i]);

    while (!queue.empty()) {
        tail->next = queue.top();
        queue.pop();
        tail = tail->next;

        if (tail->next) {
            queue.push(tail->next);
        }
    }

    return dummy->next;
}
```

### Misc
```cpp
ListNode *Solution::mergeKLists(vector<ListNode *> &A) {
    map<int, int> m;

    for (int i = 0; i < A.size(); i++) {
        ListNode *curr = A[i];
        while (curr != NULL) {
            int temp = curr->val;
            if (m.find(temp) != m.end()) {
                m[temp]++;
            } else {
                m[temp] = 1;
            }
            curr = curr->next;
        }
    }

    auto it = m.begin();

    ListNode *head = NULL;
    ListNode *curr = NULL;

    while (it != m.end()) {
        while (it->second != 0) {
            ListNode *list = new ListNode(it->first);
            if (head == NULL) {
                head = list;
                curr = list;
            } else {
                curr->next = list;
                curr = curr->next;
            }
            it->second--;
        }
        it++;
    }

    return head;
}
```

## Asked in
* Flipkart
* Amazon
* Google

