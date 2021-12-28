# 474. Ones and Zeroes

### Problem:

In the computer world, use restricted resource you have to generate maximum benefit is what we always want to pursue.

For now, suppose you are a dominator of m 0s and n 1s respectively. On the other hand, there is an array with strings consisting of only 0s and 1s.

Now your task is to find the maximum number of strings that you can form with given m 0s and n 1s. Each 0 and 1 can be used at most once.

Note:  
The given numbers of 0s and 1s will both not exceed 100  
The size of given string array won't exceed 600.  
Example 1:

```
Input: Array = {"10", "0001", "111001", "1", "0"}, m = 5, n = 3
Output: 4

Explanation: This are totally 4 strings can be formed by the using of 5 0s and 3 1s, which are “10,”0001”,”1”,”0”
```

Example 2:

```
Input: Array = {"10", "0", "1"}, m = 1, n = 1
Output: 2

Explanation: You could form "10", but then you'd have nothing left. Better form "0" and "1".
```

### Solutions:

```java
For each string in the set, we have the choice to include it in the subset or leave it. For Max(strs.length) == 600,
 we would have 2^600, different ways of choosing and we have to find the way which maximizes the subset size. 
 Obviously, this exponential Time complexity brute force approach won't work.

✔️ Solution (Dynamic Programming)

This problem looks a lot like the knapsack just that we have two constraints here m and n instead of just W in the
 knapsack problem. Here, a set of items(strings) are given and we have to choose a subset satisfying given constraints.
  We can apply DP here. We can maintain a 2d dp array, where dp[i][j] will maintain the optimal solution when
   zeros_limit = i & ones_limit = j.

For each string, some number of 0s(lets call zeros) and 1s (lets call ones) are required. Obviously, if our balance of 
zeros and ones is less than what is required by current string, we can't choose it. But in the case where our balance
 of zeros and ones is greater than the required, we have two cases -

Either take the current string into our subset. The resultant count would be 1 + optimal solution that we had when our
 balance was i - zeros & j - ones.
Or leave the current string meaning the resultant count will remain the same.
For each string in strs, we will update the dp matrix as per the above two cases.

int findMaxForm(vector<string>& strs, int m, int n) {
	// dp[i][j] will store Max subset size possible with zeros_limit = i, ones_limit = j
	vector<vector<int> > dp(m + 1, vector<int>(n + 1));
	for(auto& str : strs) {
		// count zeros & ones frequency in current string            
		int zeros = count(begin(str), end(str), '0'), ones = size(str) - zeros; 
		// which positions of dp will be updated ?
		// Only those having atleast `zeros` 0s(i >= zeros) and `ones` 1s(j >= ones)
		for(int i = m; i >= zeros; i--)
			for(int j = n; j >= ones; j--)                    
				dp[i][j] = max(dp[i][j], // either leave the current string
                            dp[i - zeros][j - ones] + 1); 
// or take it by adding 1 to optimal solution of remaining balance
// at this point each dp[i][j] will store optimal value for items considered till now & having constraints i and j respectively
	}
	return dp[m][n];
}
```



