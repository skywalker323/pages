# 147 Insertion Sort List – Medium

### Problem:

Sort a linked list using insertion sort.

### Thoughts:

This is a very basic sorting problem.

If you have learnt algorithms, this could be the very first algorithm you learnt.

Only difference is instead of an array, it is using a Single Linked List which complicates the problem a little bit. Because swapping two elements in a LinkedList is not that easy as in array.

The solution below is still in O\(n^2\) which is the standard insertion sort running time.

### Solutions:

```cpp
class Solution {
public:
    ListNode* insertionSortList(ListNode* head) {
        if(!head||!head->next) return head;

        ListNode* temp = head;
        ListNode* next = nullptr;
        ListNode* sorted = nullptr;

        while(!temp){
            next = temp->next;
            // apply insrtion sort logic
            if(!sorted || sorted->val > temp->val){
                temp->next = sorted;
                sorted = temp;
            }
            else{
                ListNode* curr = sorted;
                while(curr->next  && curr->next->val<temp->val){
                    curr = curr->next;
                }
                temp->next = curr->next;
                curr->next = temp;
            }
            temp = next;
        }
        head = sorted;
        return head;
    }
};
```



