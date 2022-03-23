# 24 Swap Nodes in Pairs – Medium

### Problem:

Given a linked list, swap every two adjacent nodes and return its head.

For example,  
Given 1-&gt;2-&gt;3-&gt;4, you should return the list as 2-&gt;1-&gt;4-&gt;3.

Your algorithm should use only constant space. You may not modify the values in the list, only nodes itself can be changed.

### Thoughts:

Very basic iteration of the list.

### Solutions:

```java
ListNode* swapPairs(ListNode* head) {
        if(!head) return nullptr;
        ListNode dummy;
        ListNode* pre = &dummy;
        ListNode* cur = head;
        ListNode* temp = nullptr;
        while(cur and cur->next)
        {
            temp = cur->next->next;
            pre->next = cur->next;
            pre->next->next = cur;
            pre = cur;
            cur = temp;
        }
        
       if(cur) { pre->next = cur; pre = pre->next;}
       pre->next = nullptr;
        
       return dummy.next;     
    }
```



