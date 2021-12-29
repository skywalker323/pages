# 322. Coin Change

### Problem:

You are given coins of different denominations and a total amount of money amount. Write a function to compute the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1.

Example 1:  
coins = \[1, 2, 5\], amount = 11  
return 3 \(11 = 5 + 5 + 1\)

Example 2:  
coins = \[2\], amount = 3  
return -1.

Note:  
You may assume that you have an infinite number of each kind of coin.

### Solutions:

```java
public class Solution {
    public int coinChange(int[] coins, int amount) {
        int[] mins = new int[amount + 1];
        for (int i = 1; i <= amount; i ++) {
            int min = Integer.MAX_VALUE;
            for (int j = 0; j < coins.length; j ++) {
                if (i - coins[j] >= 0 && mins[i - coins[j]] != Integer.MAX_VALUE) {
                    min = Math.min(min, mins[i - coins[j]] + 1);
                }   
            }
            mins[i] = min;
        }
        if (mins[amount] == Integer.MAX_VALUE) {
            return -1;
        }
        return mins[amount];
    }
}


/*The official solution is very good, I just want to share the 1D knapsack approach similar to Coin Change II.

Now lets first look at the 2D DP table and induction.

DP[i][j] means the minimum number of coins needed to achieve amount j using first i coins.

When i = 0, we only use coin with value 1.
When i = 1, we use coin with value 1 or 2.
When i = 2, we use coin with value 1, 2, or 5.

DP[i][j] = min(DP[i - 1][j], DP[i][j - coin[i]] + 1); 

      0 1 2 3 4 5 6 7 8 9 10 11
1     0 1 2 3 4 5 6 7 8 9 10 11 
1,2   0 1 1 2 2 3 3 4 4 5  5  6
1,2,5 0 1 1 2 2 1 2 2 3 3  2  3
Now lets optimize it into 1D DP approach.

Here DP[j] means the mininum number of coins required to achieve the amount j.

Then we iterate through each coin, for each outer for loop, we are trying to find the minimum number 
of coins required to reach amount j using first i coins.
*/
    int coinChange(vector<int>& coins, int amount) {             
    //Solution likes Coin Change II
        vector<int> DP(amount + 1, amount + 1);
        DP[0] = 0;
        for (int i = 0; i < coins.size(); ++i) {
            int coin_val = coins[i];
            for (int j = 1; j <= amount; ++j) {
                if (j - coin_val < 0) continue;
                DP[j] = min(DP[j], DP[j - coin_val] + 1);
            }
        }
        return DP[amount] <= amount ? DP[amount] : -1;
    }
```



