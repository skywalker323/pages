# 25 Reverse Nodes in k-Group

### Problem

Given a linked list, reverse the nodes of a linked list k at a time and return its modified list.

k is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of k then left-out nodes in the end should remain as it is.

You may not alter the values in the nodes, only nodes itself may be changed.

Only constant memory is allowed.

For example,  
Given this linked list: 1-&gt;2-&gt;3-&gt;4-&gt;5

For k = 2, you should return: 2-&gt;1-&gt;4-&gt;3-&gt;5

For k = 3, you should return: 3-&gt;2-&gt;1-&gt;4-&gt;5

### Solution

```java
 ListNode* reverseKGroup(ListNode* head, int k) {
        ListNode* prevptr = NULL;
        ListNode* currptr = head;
        ListNode* nextptr;
        int count =0;
        
        ListNode * nextHead = head;
        for (int i=0; i<k; ++i) {
          if (!nextHead)
          {
              return head;
          }
          nextHead = nextHead->next;
    }
        
        while(currptr!=NULL && count<k){
            nextptr = currptr->next;
            currptr->next=prevptr;
            prevptr = currptr;
            currptr = nextptr;
            count++;
            
        }
        if(nextptr!= NULL){
        head->next = reverseKGroup(nextptr,k);
        }
        return prevptr;
    }
```



