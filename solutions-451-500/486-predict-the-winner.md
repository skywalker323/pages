# 486 Predict the Winner

### Problem:

Given an array of scores that are non-negative integers. Player 1 picks one of the numbers from either end of the array followed by the player 2 and then player 1 and so on. Each time a player picks a number, that number will not be available for the next player. This continues until all the scores have been chosen. The player with the maximum score wins.

Given an array of scores, predict whether player 1 is the winner. You can assume each player plays to maximize his score.

Example 1:

```
Input: [1, 5, 2]
Output: False
Explanation: Initially, player 1 can choose between 1 and 2. 
If he chooses 2 (or 1), then player 2 can choose from 1 (or 2) and 5. If player 2 chooses 5, then player 1 will be left with 1 (or 2). 
So, final score of player 1 is 1 + 2 = 3, and player 2 is 5. 
Hence, player 1 will never be the winner and you need to return False.
```

### Solutions:

```java
public class Solution {
    public boolean PredictTheWinner(int[] nums) {
        int[][] gain = new int[nums.length][nums.length];
        for (int k = 0; k < nums.length; k ++) {
            for (int i = 0; i + k < nums.length; i ++) {
                if (k == 0) {
                    gain[i][i + k] = nums[i];
                }
                else if (k == 1) {
                    gain[i][i + k] = Math.abs(nums[i] - nums[i + k]);
                }
                else {
                    gain[i][i + k] = Math.max(gain[i][i] - gain[i + 1][i + k], gain[i + k][i + k] - gain[i][i + k - 1]);
                }
            }
        }
        return gain[0][nums.length - 1] >= 0;
    }
}
```

```java


Store the maximum score player1 can get for any sub array [i, j]

Given an array A[i, j], player 1 can either take the first number A[i] or A[j], after that, it forms a new arrayA[i+1, j] or A[i, j-1] accordingly and it is player2's turn to pick up. The maximum score that player1 can get from the the sub arrays will be the larger one left by player2. So,

DP formula:
dp(i, j) = max(sum(i, j-1) - dp(i, j-1) + nums[j], sum(i+1, j) - dp(i+1, j) + nums[i])

Because sum(i, j-1) + nums[j] = sum(i, j) = nums[i] + sum(i+1, j), the formula can be simplified to
dp(i, j) = max(sum(i, j) - dp(i, j-1), sum(i, j) - dp(i+1, j))

More simpler:
From dp(i, j) = max(sum(i, j) - dp(i, j-1), sum(i, j) - dp(i+1, j)), each dp(i, j) contains sum(i, j), then if we subtract the sum, it should have no impact to final result.

Thanks to @coder2 point out that the mistake of the dp formula deduction.
If we do more deduction, we can eliminate the sum(i, j) from the formula:
Instead of storing the maximum score that player 1 can get in each sub array, we can store the diff between player1 and player 2. For example: if player 1 get A, player 2 get B, we can use dp' to store A-B.

if A = dp(i, j), then B = sum(i, j) - dp(i, j)

So dp'(i, j) = dp(i, j) - ( sum(i, j) - dp(i, j) ) = 2*dp(i, j) - sum(i, j), so
2*dp(i, j) = dp'(i, j) + sum(i, j) (this will be used below)

dp'(i, j) = dp(i, j) - ( sum(i, j) - dp(i, j) ) = 2dp(i, j) - sum(i, j)
= 2 * max( sum(i, j) - dp(i, j-1), sum(i, j) - dp(i+1, j) ) - sum(i, j)
= max(sum(i, j) - 2*dp(i, j-1), sum(i, j) - 2*dp(i+1, j) )
= max(sum(i, j) - ( dp'(i, j-1) + sum(i, j-1) ), sum(i, j) - ( dp'(i+1, j) + sum(i+1, j)))
= max(sum(i, j) - sum(i, j-1) - dp'(i, j-1), sum(i, j) - sum(i+1, j) - dp'(i+1, j))
= max(nums[j] - dp'(i, j-1), nums[i] - dp'(i+1, j))

Final formula: dp(i, j) = max(nums[j] - dp(i, j-1), nums[i] - dp(i+1, j))

class Solution {
public:
    bool PredictTheWinner(vector<int>& nums) {
        int n = nums.size();
        vector<vector<int>> dp(n, vector<int>(n)); // use to keep the score gap between player1 and player2
        for (int i = 0; i < n; i++) dp[i][i] = nums[i];
        for (int i = 1; i < n; i++) {
            for (int j = 0; j+i < n; j++) {
                dp[j][j+i] = max(nums[j+i]-dp[j][j+i-1], nums[j]-dp[j+1][j+i]);
            }
        }
        return dp[0][n-1] >= 0; // player1 get more score points than player2
    }
};
```



