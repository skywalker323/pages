# 143 Reorder List – Medium

### Problem:

Given a singly linked list L: L0→L1→…→Ln-1→Ln,  
reorder it to: L0→Ln→L1→Ln-1→L2→Ln-2→…

You must do this in-place without altering the nodes’ values.

For example,  
Given {1,2,3,4}, reorder it to {1,4,2,3}.

### Thoughts

This problem can be easily solved by using HashMap, but due to the requirement, “do this in-place”.  
So the plan is to complete in three steps.  
1 Find the middle element of the list.  
2 Do a reverse operation on the second half of the list.  
3 Merge the first half and second half in a fashion that picks element from each in order.

### Solutions:

```java

class Solution {
public:
    // O(1) space
    void reorderList(ListNode* head) {

        // find middle
        ListNode* dummy = new ListNode(0);
        dummy->next = head;
        ListNode* fast = dummy, *slow = dummy;

        while (fast && fast->next)
        {
            slow = slow->next;
            fast = fast->next->next;
        }

        ListNode* middle = slow->next;
        slow->next = nullptr;

        // reverse middle list
        ListNode* prev = nullptr, *next = nullptr, *curr = middle;
        while (curr)
        {
            next = curr->next;
            curr->next = prev;
            prev = curr;
            curr = next;
        }

        // iterate over two list
        ListNode* curr1 = head, *curr2 = prev;
        while (curr1 && curr2)
        {
            ListNode* tempNext = curr1->next;
            ListNode* tempPrev = curr2->next;

            curr1->next = curr2;
            curr2->next = tempNext;

            curr1 = tempNext;
            curr2 = tempPrev;
        }       
    }
};


class Solution {
public:
    void reorderList(ListNode* head) 
    {
        ListNode *curr, *next;
        stack<ListNode*> stk;
        int count = 0;
        ListNode *temp = head;
        
        while(temp)
        {
            stk.push(temp);
            temp = temp->next;
            count++;
        }

        curr = head;
       
        for(int i = 0; i < count/2; i++)
        {
            next = curr->next;
            temp = stk.top();
            curr->next = temp;
            temp->next = next;
            curr = next;
            stk.pop();
        }
        
        curr->next = NULL;
    }
};




```



