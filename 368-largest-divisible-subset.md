# 368 Largest Divisible Subset

### Problem:

Given a set of distinct positive integers, find the largest subset such that every pair \(Si, Sj\) of elements in this subset satisfies: Si % Sj = 0 or Sj % Si = 0.

If there are multiple solutions, return any subset is fine.

Example 1:

```
nums: [1,2,3]

Result: [1,2] (of course, [1,3] will also be ok)
```

Example 2:

```
nums: [1,2,4,8]

Result: [1,2,4,8]
```

### Solutions:

```java
class Solution {
public:
    vector<int> largestDivisibleSubset(vector<int>& nums) {
        vector<pair<int, int>> dp(nums.size(), {1, -1}); // set size, previous tracing index
        vector<int> res;

        int maxIndex = 0;
        int maxSize = 1;

        // sort the array because of element with common divisor does not
        // mean that ther are divisible
        sort(nums.begin(), nums.end());

        // for each element in nums
        for (int i = 1; i < nums.size(); i++) {

            // for each element before i, check if it is divisible
            for (int j = 0; j < i; j++) {

                // divisible with a larger subset size, dp it
                if (nums[i] % nums[j] == 0 && dp[j].first + 1 > dp[i].first) {
                    dp[i].first = dp[j].first + 1;
                    dp[i].second = j;
                }
            }

            // mark down the starting point of the current largest subset
            if (dp[i].first > maxSize) {
                maxSize = dp[i].first;
                maxIndex = i;
            }
        }

        // Rebuild one of the subsets, order is not important
        while (maxIndex >= 0) {
            res.push_back(nums[maxIndex]);
            maxIndex = dp[maxIndex].second;
        }

        return res;
    }
};
```



```java
The code is very similar to the traditional LIS. I have used an extra array child to track predecessors of indices. Hence, dp[i] will tell the length of the Longest Divisible Subset ending at the ith index, and child[i] will tell the index which comes before the ith index in the subset which includes the ith index. child[i] will be -1, if the ith index is the first element in the subset.

C++

class Solution {
public:
    vector<int> largestDivisibleSubset(vector<int>& nums) {
        
        // Sorting: to leverage the transitive property!
        sort(nums.begin(), nums.end());
        int n = nums.size();
        
        // Initializing the DP and CHILD arrays
        vector<int> dp(n, 1), child(n, -1);
        
        int imax = 0;
        
        for(int i = 1; i < n; i++) {
            
            // Considering ith element as the last element
            // and finding the largest subset it can 
            // belong to
            for(int j = 0; j < i; j++) {
                
                // Using transitivity, if nums[i] % nums[j] == 0
                // then all numbers in the subset ending at j
                // will divide num[i] as well!
                if(nums[i] % nums[j] == 0) {
                    
                    // Inclusion of i will increase the size of 
                    // the subset by 1
                    if(1 + dp[j] > dp[i]) {
                        dp[i] = 1 + dp[j];
                        
                        // Setting the predecessor of i
                        child[i] = j;
                    }
                }
            }
            
            // Determining the index where the largest subset ends
            if(dp[i] > dp[imax]) {
                imax = i;
            }
        }
        
        vector<int> ans;
        
        // Backtracking to obtain the entire largest subset!
        // This condition makes sure that the loop stops
        // once the first element of the subset is traversed
        while(imax != -1) {
            ans.push_back(nums[imax]);
            imax = child[imax];
        }
        
        return ans;
    }
};
```



