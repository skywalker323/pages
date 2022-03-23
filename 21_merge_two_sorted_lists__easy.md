# 21 Merge Two Sorted Lists – Easy

### Problem:

Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.

### Thoughts:

This is a very straightforward problem. Using a fakeHead pointer is solving a lot of troubles in many list problems. For this problem, you don’t have to use it. You could also solve the problem without using it.

### Solutions:

```java
ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
        ListNode *dummy, *temp;
        dummy = new ListNode();
        temp = dummy;
        
        //when both list1 and list2 isn't empty
        while(list1 && list2){
            if(list1->val < list2->val){
                temp->next = list1;
                list1 = list1->next;
            }
            else{
                temp->next = list2;
                list2 = list2->next;   
            }
            temp = temp->next;
        }
        
        // we reached at the end of one of the list
        if(list1) temp->next = list1;
        if(list2) temp->next = list2;
        
        return dummy->next;
    }
```



