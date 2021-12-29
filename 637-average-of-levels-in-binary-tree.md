# 637 Average of Levels in Binary Tree

### Problem

Given a non-empty binary tree, return the average value of the nodes on each level in the form of an array.

Example 1:

```
Input:
    3
   / \
  9  20
    /  \
   15   7
Output: [3, 14.5, 11]
```

Explanation:  
The average value of nodes on level 0 is 3,  on level 1 is 14.5, and on level 2 is 11. Hence return \[3, 14.5, 11\].  
Note:  
The range of node's value is in the range of 32-bit signed integer.

### Solutions

```java
We can do a simple depth-first traversal on Tree(preOrder traveral in below solution) and maintain a hashmap to store level of node, number of nodes on that level and the corresponding sum of nodes on same level. Lastly, iterate the hashmap to create the final array containing the average of nodes on each level by dividing sum of nodes on a level by number of nodes.

vector<double> averageOfLevels(TreeNode* root) {       
	// mp[i] will map to ith level and contain a pair which will store - {number of nodes, sum}
	unordered_map<int, pair<int, long> >mp; 
	solve(root, mp, 0);
	vector<double> ans(mp.size());
	for(int i=0;i<ans.size();i++)
		ans[i] = (double)mp[i].second / mp[i].first;
	return ans;
}
void solve(TreeNode* root, unordered_map<int, pair<int, long> >&mp, int lvl){
	if(!root)return;
//   level     number of nodes,  sum of nodes of same level
	mp[lvl] = {mp[lvl].first + 1, mp[lvl].second + root -> val};
	solve(root -> left, mp, lvl + 1);
	solve(root -> right, mp, lvl + 1);
}

Time Complexity : O(N), where N is the number of nodes in the given Tree.
Space Complexity : O(H), where H is the maximum height or number of levels in the given Tree.

Solution - II (BFS)
We can directly do a Breadth-First search, which would be more intuitive and efficient than DFS, since we are concerned with each levels of the binary Tree(exactly what BFS achieves).

As in any BFS, we use a simple queue, iterate nodes at each row & push their children to queue after popping the current node. For each row, sum of all node's value at that level of the binary tree would be calculated and the average would be stored in ans array which will be our final answer.

vector<double> averageOfLevels(TreeNode* root) {       
	vector<double>ans;
	queue<TreeNode* > q;
	q.push(root);
	// while there are nodes remaining to be visited
	while(!q.empty()){            
		int n = q.size();
		long sum = 0;
		// iterating each level of binary tree
		for(int i = 0; i < n; i++){
			TreeNode* top = q.front(); q.pop();
			sum += top -> val;
			if(top -> left) q.push(top -> left);
			if(top -> right) q.push(top -> right);
		}
		ans.push_back((double)sum / n);
	}
	return ans;
}
Time Complexity : O(N), where N is the number of nodes in the given Tree.
Space Complexity : O(M), where M is the maximum number of nodes at any level in the binary tree.

Note, in the BFS, we are only storing a pointer, unlike in DFS case where we had to store level, number of nodes and sum. So, memory usage would be much smaller here.
```



