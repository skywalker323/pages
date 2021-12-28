# 444. Sequence Reconstruction

### Problem:

Check whether the original sequence org can be uniquely reconstructed from the sequences in seqs. The org sequence is a permutation of the integers from 1 to n, with 1 ≤ n ≤ 104. Reconstruction means building a shortest common supersequence of the sequences in seqs \(i.e., a shortest sequence so that all sequences in seqs are subsequences of it\). Determine whether there is only one sequence that can be reconstructed from seqs and it is the org sequence.

Example 1:

```
Input:
org: [1,2,3], seqs: [[1,2],[1,3]]

Output:
false

Explanation:
[1,2,3] is not the only one sequence that can be reconstructed, because [1,3,2] is also a valid sequence that can be reconstructed.
```

Example 2:

```
Input:
org: [1,2,3], seqs: [[1,2]]

Output:
false

Explanation:
The reconstructed sequence can only be [1,2].
```

Example 3:

```
Input:
org: [1,2,3], seqs: [[1,2],[1,3],[2,3]]

Output:
true

Explanation:
The sequences [1,2], [1,3], and [2,3] can uniquely reconstruct the original sequence [1,2,3].
```

Example 4:

```
Input:
org: [4,1,5,2,6,3], seqs: [[5,2,6,3],[4,1,5,2]]

Output:
true
```

### Solutions:

```java
For org to be uniquely reconstructible from seqs we need to satisfy 2 conditions:

Every sequence in seqs should be a subsequence in org. This part is obvious.
Every 2 consecutive elements in org should be consecutive elements in some sequence from seqs. Why is that? Well, suppose condition 1 is satisfied. Then for 2 any consecutive elements x and y in org we have 2 options.
We have both xand y in some sequence from seqs. Then (as condition 1 is satisfied) they must be consequtive elements in this sequence.
There is no sequence in seqs that contains both x and y. In this case we cannot uniquely reconstruct org from seqs as sequence with x and y switched would also be a valid original sequence for seqs.
So this are 2 necessary criterions. It is pretty easy to see that this are also sufficient criterions for org to be uniquely reconstructible (there is only 1 way to reconstruct sequence when we know that condition 2 is satisfied).

To implement this idea I have idxs hash that maps item to its index in org sequence to check condition 1. And I have pairs set that holds all consequitive element pairs for sequences from seqs to check condition 2 (I also consider first elements to be paired with previous undefined elements, it is necessary to check this).

var sequenceReconstruction = function(org, seqs) {
    const pairs = {};
    const idxs = {};
    
    for (let i = 0; i < org.length; i++)
        idxs[org[i]] = i;

    for (let j = 0; j < seqs.length; j++) {
        const s = seqs[j];
        for (let i = 0; i < s.length; i++) {
            if (idxs[s[i]] == null)
                return false;
            if (i > 0 && idxs[s[i - 1]] >= idxs[s[i]])
                return false;
            pairs[`${s[i - 1]}_${s[i]}`] = 1;
        }
    }

    for (let i = 0; i < org.length; i++)
        if (pairs[`${org[i - 1]}_${org[i]}`] == null)
            return false;

    return true;
};
```

```java
class Solution {
public:
    bool sequenceReconstruction(vector<int>& org, vector<vector<int>>& seqs) {
        if (seqs.size() == 0) return false;
        int n = org.size(), count = 0;
        unordered_map<int, unordered_set<int>> graph;   // record parents
        vector<int> degree(n+1, 0); // record out degree
        for (auto s : seqs) {   // build graph
            for (int i = s.size()-1; i >= 0; --i) {
                if (s[i] > n or s[i] < 0) return false; // in case number in seqs is out of range 1-n
                if (i > 0 and !graph[s[i]].count(s[i-1])) {
                    graph[s[i]].insert(s[i-1]);
                    if (degree[s[i-1]]++ == 0) count ++;
                }
            }
        }
        if (count != n-1) return false; // all nodes should have degree larger than 0 except the last one
        for (int i = n-1; i >= 0; --i) {    // topological sort
            if (degree[org[i]] > 0) return false;   // the last node should have 0 degree
            for (auto p : graph[org[i]]) 
                if (--degree[p] == 0 and p != org[i-1]) // found a node that is not supposed to have 0 degree
                    return false;
        }
        return true;
    }
};
```

```java
public class Solution {
    public boolean sequenceReconstruction(int[] org, List<List<Integer>> seqs) {
        HashMap<Integer, Integer> index = new HashMap<Integer, Integer>();
        boolean[] checked = new boolean[org.length];
        for (int i = 0; i < org.length; i ++) {
            index.put(org[i], i);
        }
        boolean hasChecked = false;
        int toCheck = org.length - 1;
        for (List<Integer> seq:seqs) {
            for (int i = 0; i < seq.size(); i ++) {
                hasChecked = true;
                int curr = seq.get(i);
                if (curr <= 0 || curr > org.length) {
                    return false;
                }
                if (i == 0) {
                    continue;
                }
                int pre = seq.get(i - 1);
                if (index.get(pre) >= index.get(curr)) {
                    return false;
                }
                if (checked[index.get(curr)] == false && index.get(pre) == index.get(curr) - 1) {
                    checked[index.get(curr)] = true;
                    toCheck --;
                }
            }
        }
        return toCheck == 0 && hasChecked;
    }
}
```



