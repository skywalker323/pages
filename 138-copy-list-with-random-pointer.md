# 138 Copy List with Random Pointer

### Problem

A linked list is given such that each node contains an additional random pointer which could point to any node in the list or null.

Return a deep copy of the list.

### Solutions

```cpp
     Node* copyRandomList(Node* head) {
        Node * clone = head; // create a clone 
        unordered_map<Node*, Node*>m; // use map method
        
        
        // the key is head, the value is only the value of head without address
        while(clone){
            Node* node= new Node(clone->val);// create a new node
            m[clone] = node;
            clone = clone->next;
        }
        
        Node* res = new Node(-1); // set default as null
        clone = head; // set it equal to head again
        while(head){
            res = m[head]; // let res be the value
            
            // find the corresponding values of random and next respectively
            //////////////////////////////            
            res->random = m[head->random]; 
            res->next = m[head->next];
            //////////////////////////////
            res = res->next;
            head = head->next;
        }
        return m[clone];
    } 
 
```





