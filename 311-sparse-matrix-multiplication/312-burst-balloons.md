# 312 Burst Balloons

### Problem:

Given n balloons, indexed from 0 to n-1. Each balloon is painted with a number on it represented by array nums. You are asked to burst all the balloons. If the you burst balloon i you will get nums\[left\] _ nums\[i\] _ nums\[right\] coins. Here left and right are adjacent indices of i. After the burst, the left and right then becomes adjacent.

Find the maximum coins you can collect by bursting the balloons wisely.

Note:  
\(1\) You may imagine nums\[-1\] = nums\[n\] = 1. They are not real therefore you can not burst them.  
\(2\) 0 ≤ n ≤ 500, 0 ≤ nums\[i\] ≤ 100

Example:

Given \[3, 1, 5, 8\]

Return 167

```
    nums = [3,1,5,8] --> [3,5,8] -->   [3,8]   -->  [8]  --> []
   coins =  3*1*5      +  3*5*8    +  1*3*8      + 1*8*1   = 167
```

### Solutions:

```cpp
/*This question is similar to matrix chain multiplication.
If you think of bursting a balloon as multiplying two adjacent matrices, then this problem is exactly the 
classical DP problem Matrix-chain multiplication found in section 15.2 in the book Introduction to Algorithms 
(2nd edition).

For example, given [3,5,8] and bursting 5, the number of coins you get is the number of scalar multiplications 
you need to do to multiply two matrices A[35] and B[58]. So in this example, the original problem is actually the
 same as given a matrix chain A[1*3]B[35]C[58]D[81], fully parenthesize it so that the total number of scalar 
 multiplications is maximized, although the orignal matrix-chain multiplication problem in the book asks to 
 minimize it. Then you can see it clearly as a classical DP problem.
*/

int dp[502][502];
int solve(vector<int>& nums, int i, int j){
        if(i>=j) return 0;
        if(dp[i][j] != -1) return dp[i][j];
        int ans = INT_MIN;
        for(int k = i; k<=j-1; k++){
            int temp = solve(nums,i,k) + solve(nums,k+1,j) + nums[k]*nums[i-1]*nums[j];
            ans = max(ans,temp);
        }
        return dp[i][j] = ans;
}
int maxCoins(vector<int>& nums) {
        nums.insert(nums.begin(),1);
        nums.insert(nums.end(),1);
        memset(dp,-1,sizeof dp);
        return solve(nums,1,nums.size()-1); 
}
```



