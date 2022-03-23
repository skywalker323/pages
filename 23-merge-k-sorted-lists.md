# 23 Merge k Sorted Lists

### Problem:

Merge k sorted linked lists and return it as one sorted list. Analyze and describe its complexity.

### Solutions:

```java
ListNode* mergeKLists(vector<ListNode*>& lists) {

        ListNode* node = new ListNode();
        ListNode* head = node;
        multiset<pair<int, ListNode*>> ms;
        for (auto j : lists)
        {
            if (j != NULL)
                ms.insert({ j->val, j });
        }
        while (!ms.empty())
        {
            auto i = ms.begin();
            node->next = new ListNode(i->first); //
            node = node->next;
            if (i->second->next)
                ms.insert({ i->second->next->val,i->second->next });
            ms.erase(i);
        }
        return head->next;

    }
```



