# 328. Odd Even Linked List

### Problem:

Given a singly linked list, group all odd nodes together followed by the even nodes. Please note here we are talking about the node number and not the value in the nodes.

You should try to do it in place. The program should run in O\(1\) space complexity and O\(nodes\) time complexity.

Example:  
Given 1-&gt;2-&gt;3-&gt;4-&gt;5-&gt;NULL,  
return 1-&gt;3-&gt;5-&gt;2-&gt;4-&gt;NULL.

Note:  
The relative order inside both the even and odd groups should remain as it was in the input.   
The first node is considered odd, the second node even and so on ...

### Solutions:

```java
APPROACH :

/*
The idea is to split the linked list into 2 : one containing all odd nodes and other containing all even nodes. And finally, attach the even node linked list at the end of the odd node linked list.
To split the linked list into even nodes & odd nodes, we traverse the linked list and keep connecting the consecutive odd nodes and even nodes (to maintain the order of nodes) and save the pointer to the first even node.
Finally, we insert all the even nodes at the end of the odd node list.
image */




class Solution {
public:
    ListNode* oddEvenList(ListNode* head) {
        if(!head || !head->next || !head->next->next) return head;
        
        ListNode *odd = head;
        ListNode *even = head->next;
        ListNode *even_start = head->next;
        
        while(odd->next && even->next){
            odd->next = even->next; //Connect all odds
            even->next = odd->next->next;  //Connect all evens
            odd = odd->next;
            even = even->next;
        }
        odd->next = even_start;   //Place the first even node after the last odd node.
        return head; 
    }
};
```



