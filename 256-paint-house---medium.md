# 256. Paint House - Medium

### Problem:
<pre>
There are a row of n houses, each house can be painted with one of the three colors: red, blue or green. The cost of painting each house with a certain color is different. You have to paint all the houses such that no two adjacent houses have the same color.

The cost of painting each house with a certain color is represented by a n x 3 cost matrix. For example, costs[0][0] is the cost of painting house 0 with color red; costs[1][2] is the cost of painting house 1 with color green, and so on... Find the minimum cost to paint all houses.

Note:
All costs are positive integers.
</pre>

### Solutions:

```
class Solution {
public:
    int minCost(vector<vector<int>>& costs) {
        if( costs.empty()) return 0;

        vector<vector<int>> dp(costs.size(), vector<int>(costs[0].size(), 0));

        for(int i=0; i < costs[0].size(); i++)
            dp[0][i] = costs[0][i];

        for(int i =1; i < costs.size(); i++)
        {
            dp[i][0] = min(dp[i-1][1], dp[i-1][2]) + costs[i][0];
            dp[i][1] = min(dp[i-1][0], dp[i-1][2]) + costs[i][1];
            dp[i][2] = min(dp[i-1][0], dp[i-1][1]) + costs[i][2];
        }


        return min(min(dp[costs.size()-1][0], dp[costs.size()-1][1]) , dp[costs.size()-1][2]);
    }
};
```
