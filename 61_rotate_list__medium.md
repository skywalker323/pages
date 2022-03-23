# 61 Rotate List – Medium

### Problem:

Given a list, rotate the list to the right by k places, where k is non-negative.

For example:  
Given 1-&gt;2-&gt;3-&gt;4-&gt;5-&gt;NULL and k = 2,  
return 4-&gt;5-&gt;1-&gt;2-&gt;3-&gt;NULL.

### Thoughts:

Here we are going to use the approach that calculates the length of the list first, then get the offset and do the rotation.

If the input offset is always going to be valid, a fast and slow pointer way is a little better. But in the Leetcode version, k could be larger than the length, so we need the length anyway.

### Solutions:

```java
 ListNode* rotateRight(ListNode* head, int k) {
        
        if(!head) return head;
        ListNode* cur = head;
        ListNode* prev;
        int i =0;
        while(cur)
        {
            prev = cur;
            cur = cur->next;
            i++;
        }
        if(! k % i) return head;
        // prev is end
        prev->next = head;
        i = k > i ? i- (k %i) : i-k;
        
        while(i)
        {
           i--;
           prev = prev->next; 
        }
        head = prev->next;
        prev->next = nullptr;
        return head;
    }
```



