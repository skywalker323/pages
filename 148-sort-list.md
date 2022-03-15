# 148 Sort List

### Problem:

Sort a linked list in O\(n log n\) time using constant space complexity.

### Solutions:

```cpp

ListNode *Merge(ListNode *n1,ListNode *n2)
{
    ListNode *curr=new ListNode (0);
    if(!n1) return n2;
    if(!n2) return n1;
    
    if(n1->val<=n2->val)
    {
        curr->val=n1->val;
        curr->next=Merge(n1->next,n2);
        
    }else
    {
        curr->val=n2->val;
        curr->next=Merge(n1,n2->next);
    }
    
    return curr;
}

ListNode* sortList(ListNode* head) {
    ListNode *curr=head;
    if(!curr  or !curr->next) return curr;
    
    ListNode *temp = NULL;
    ListNode *mid = head;
    ListNode *end = head;
    
    while(end !=  NULL && end -> next != NULL)
    {
        temp = mid;
        mid = mid->next;          
        end = end ->next ->next;  
        
    }   
    temp -> next = NULL; 
    ListNode *n1=sortList(head);
    ListNode *n2=sortList(mid);

    return Merge(n1, n2); 
}
```



