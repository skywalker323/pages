# 465 Optimal Account Balancing

### Problem:

A group of friends went on holiday and sometimes lent each other money. For example, Alice paid for Bill's lunch for $10. Then later Chris gave Alice $5 for a taxi ride. We can model each transaction as a tuple \(x, y, z\) which means person x gave person y $z. Assuming Alice, Bill, and Chris are person 0, 1, and 2 respectively \(0, 1, 2 are the person's ID\), the transactions can be represented as \[\[0, 1, 10\], \[2, 0, 5\]\].

Given a list of transactions between a group of people, return the minimum number of transactions required to settle the debt.

Note:

A transaction will be given as a tuple \(x, y, z\). Note that x ≠ y and z &gt; 0.  
Person's IDs may not be linear, e.g. we could have the persons 0, 1, 2 or we could also have the persons 0, 2, 6.  
Example 1:

```
Input:
[[0,1,10], [2,0,5]]

Output:
2

Explanation:
Person #0 gave person #1 $10.
Person #2 gave person #0 $5.

Two transactions are needed. One way to settle the debt is person #1 pays person #0 and #2 $5 each.
```

Example 2:

```
Input:
[[0,1,10], [1,0,1], [1,2,5], [2,0,5]]

Output:
1

Explanation:
Person #0 gave person #1 $10.
Person #1 gave person #0 $1.
Person #1 gave person #2 $5.
Person #2 gave person #0 $5.

Therefore, person #1 only need to give person #0 $4, and all debt is settled.
```

### Solutions:

```java
class Solution {
public:
    int dfs(vector<int>& loaner, vector<int>& borrower, int loaner_id, int borrow_id){
        if(loaner_id==loaner.size())return 0;
        if(loaner_id==loaner.size()-1)return borrower.size()-borrow_id;
        if(borrow_id==borrower.size()-1)return loaner.size()-loaner_id;
        int min_val=INT_MAX;
        for(int i=loaner_id;i<loaner.size();i++){
            if(loaner[i]==borrower[borrow_id])
                return 1+dfs(loaner, borrower, loaner_id+1, borrow_id+1);
//start from next person
//nxt round start from loaner_id+1, not i+1, because i depends on loaner_id actually
            if(loaner[i]>borrower[borrow_id]){
//it means that we can get money back from the current borrower, however, it's not enough for my loss, so I
// need more borrower to pay me back my money
                loaner[i]-=borrower[borrow_id];
                min_val=min(min_val, 1+dfs(loaner, borrower, loaner_id, borrow_id+1));
                loaner[i]+=borrower[borrow_id];//for backtracking
            }
            if(loaner[i]<borrower[borrow_id]){
//it means after borrower return his money to the current loaner, he still has money to return to the other loaner
//that's why we move loaner_id by 1
                borrower[borrow_id]-=loaner[i];
                min_val=min(min_val, 1+dfs(loaner, borrower, loaner_id+1, borrow_id));
                borrower[borrow_id]+=loaner[i];
            }
        }
        return min_val;
    }
    int minTransfers(vector<vector<int>>& transactions) {
        unordered_map<int, int> loan;
        for(auto t: transactions){
            loan[t[0]]-=t[2];
            loan[t[1]]+=t[2];
        }
        
        vector<int> loaner, borrower;
        for(auto l: loan){
            if(l.second>0) borrower.push_back(l.second);
            else if(l.second<0)loaner.push_back(-l.second);
        }
        return dfs(loaner, borrower, 0, 0);
    }
};
```



