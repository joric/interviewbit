# Intersection of Linked Lists

https://www.interviewbit.com/problems/intersection-of-linked-lists/

## Solution

### Editorial
```cpp
int len(ListNode *head) {
    int l = 0;
    while (head->next != NULL) {
        ++l;
        head = head->next;
    }
    return l;
}

ListNode *Solution::getIntersectionNode(ListNode *A, ListNode *B) {
    if (!A || !B)
        return NULL;
    int lenA = len(A);
    int lenB = len(B);
    int lenDiff = lenA - lenB;
    ListNode *pa = A;
    ListNode *pb = B;
    if (lenDiff > 0) {
        while (lenDiff != 0) {
            pa = pa->next;
            lenDiff--;
        }
    } else if (lenDiff < 0) {
        while (lenDiff != 0) {
            pb = pb->next;
            lenDiff++;
        }
    }
    while (pa && pb) {
        if (pa == pb)
            return pa;
        pa = pa->next;
        pb = pb->next;
    }
    return NULL;
}

```

### Fastest
```cpp
ListNode* Solution::getIntersectionNode(ListNode* A, ListNode* B) {
  if(A==NULL || B==NULL)
  return NULL;
  ListNode *s=A;
  ListNode *t=B;
  while(s!=t){
      if(s==NULL){
          s=B;
      }
      else
        s=s->next;
    if(t==NULL)
        t=A;
    else
        t=t->next;
  }
  return s;
}
```

### Lightweight
```cpp
ListNode* Solution::getIntersectionNode(ListNode* A, ListNode* B) {
    // Do not write main() function.
    // Do not read input, instead use the arguments to the function.
    // Do not print the output, instead return values as specified
    // Still have a doubt. Checkout www.interviewbit.com/pages/sample_codes/ for more details

    ListNode *ptr;
    ptr=A;
    int len1=0;
    while(ptr!=NULL)
    {
        ptr=ptr->next;
        len1++;
    }
    ptr=B;
    int len2=0;
    while(ptr!=NULL)
    {
        ptr=ptr->next;
        len2++;
    }
    if(len1>len2)
    { 
        
        while(len1>len2)
        {
            ptr=A;
            A=A->next;
            free(ptr);
            len1--;
        }
        
        
    }
    if(len2>len1)
    {   
        while(len2>len1)
        {
            ptr=B;
            B=B->next;
            free(ptr);
            len2--;
        }
    }
    ListNode *p1,*p2;
    p1=A;
    p2=B;
    while(p1!=p2)
    {
        p1=p1->next;
        p2=p2->next;
    }
    return p1;
}

```

## Asked in

