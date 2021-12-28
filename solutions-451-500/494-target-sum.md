# 494 Target Sum

### Problem:

You are given a list of non-negative integers, a1, a2, ..., an, and a target, S. Now you have 2 symbols + and -. For each integer, you should choose one from + and - as its new symbol.

Find out how many ways to assign symbols to make sum of integers equal to target S.

Example 1:

```
Input: nums is [1, 1, 1, 1, 1], S is 3. 
Output: 5
Explanation: 

-1+1+1+1+1 = 3
+1-1+1+1+1 = 3
+1+1-1+1+1 = 3
+1+1+1-1+1 = 3
+1+1+1+1-1 = 3

There are 5 ways to assign symbols to make the sum of nums be target 3.
```

Note:  
1. The length of the given array is positive and will not exceed 20.  
2. The sum of elements in the given array will not exceed 1000.  
3. Your output answer is guaranteed to be fitted in a 32-bit integer.

### Solutions:

```java
Start from 0 to end of nums array we can count sum by taking nums[i] and -nums[i] and call
recursion. At the end of array if sum==S then return 1 otherwise 0.

As same result is calculated multiple times in recursion we can store the result of <index,sum> state
in DP vector. Max array size 20 and Max element sum 1000 and Min element sum 1000 which
makes sum range 0 to 2000. To avoid negative index we add 1000 with sum while storing and
retriving value in DP vector.
class Solution {
public:
    
    // dp vector store precalculated result of <index,sum>
    // max array size 20 and max element sum 1000 and so min element sum 1000
    // makes sum range 0 to  1000+1000 = 2000
    int dp[21][2001];
    
    // recursion from 0 to end of nums array taking +nums[i] and -nums[i] value
    int dfs(int index, vector<int>& nums, int &S, int sum)
    {
        // reached to the end of array 
        // return 1 if S==sum, otherwise return 0
        if(index==nums.size())
            return sum==S?1:0;

        // return precalculted result
        // to avoid negative index we add 1000 with sum
        if(dp[index][sum+1000]!=-1) return dp[index][sum+1000];
        
        int count = 0;
        
        // call recursion for taking both +nums[i] and -nums[i] values
        // and updated running sum as sum+nums[i] and sum-nums[i]
        
        count+= dfs(index+1,nums,S,sum+nums[index]);
        count+= dfs(index+1,nums,S,sum-nums[index]);
        
        return dp[index][sum+1000] = count;
    }
    
    int findTargetSumWays(vector<int>& nums, int S) {
        
        // set -1 to all dp values
        memset(dp,-1,sizeof(dp));
        
        // all possible ways to reach target 
        return dfs(0,nums,S,0);
    }
};
```



