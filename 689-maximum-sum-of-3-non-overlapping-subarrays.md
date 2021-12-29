# 689 Maximum Sum of 3 Non-Overlapping Subarrays

### Problem

In a given array nums of positive integers, find three non-overlapping subarrays with maximum sum.

Each subarray will be of size k, and we want to maximize the sum of all 3\*k entries.

Return the result as a list of indices representing the starting position of each interval \(0-indexed\). If there are multiple answers, return the lexicographically smallest one.

Example:

```
Input: [1,2,1,2,6,7,5,1], 2
Output: [0, 3, 5]
Explanation: Subarrays [1, 2], [2, 6], [7, 5] correspond to the starting indices [0, 3, 5].
We could have also taken [2, 1], but an answer of [1, 3, 5] would be lexicographically larger.
```

Note:  
nums.length will be between 1 and 20000.  
nums\[i\] will be between 1 and 65535.  
k will be between 1 and floor\(nums.length / 3\).

### Solutions

```java

First of all, whenever the question asks you to find the objects chosen instead of the final value you get when you choose these objects (like the indices selected in this question) you can do the DP for finding the value (like the maximum sum in this case) and do a DFS for retracing the steps.

So first of for each index find the subarray sum of length k starting at that index and store it in an array. Then simply do a knapsack DP which is like you can either choose or not choose this sub array. If you include take this sub array, then you can only go to the (i+k)th index because every in between element will have some overlap with the ith sum.

int memo[20000][4];
vector<int> ret;
int dp(int i,int n,int k,vector<int>& arr)
{
	if(!n)
		return 0;
	if(i>=arr.size())
	{
		if(n)
			return INT_MIN;
		else return 0;
	}
	else if(memo[i][n]!=-1)
		return memo[i][n];
	else
	{
		int taken=0,not_taken=0;
		not_taken=dp(i+1,n,k,arr);
		taken=arr[i]+dp(k+i,n-1,k,arr);
		return memo[i][n]=max(taken,not_taken);
	}
}
void dfs(int i,int n,int k,vector<int>& arr)
{
	if(!n)
		return;
	else
	{
		int taken=0,not_taken=0;
		taken=arr[i]+dp(i+k,n-1,k,arr);
		not_taken=dp(i+1,n,k,arr);
		if(taken>=not_taken)
		{
			ret.push_back(i);
			dfs(i+k,n-1,k,arr);
		}
		else
			dfs(i+1,n,k,arr);
	}
}
vector<int> maxSumOfThreeSubarrays(vector<int>& nums, int k) 
{
	vector<int> arr(nums.size()-k+1);
	int sum=0,j=0;
	for(int i=0;i<k;++i)
		sum+=nums[i];
	arr[j++]=sum;
	for(int i=k;i<nums.size();++i)
	{
		sum-=nums[i-k];
		sum+=nums[i];
		arr[j++]=sum;
	}
	memset(memo,-1,sizeof(memo));
	dp(0,3,k,arr);  
	dfs(0,3,k,arr);
	return ret;
}

// General solution for m non-overlap subarrays, Time and Space complexity is O(mn)
// m = 3, thus Time and Space complexity is O(n)

// Example Input: [1,2,1,2,6,7,5,1], 2

class Solution {
public:
    vector<int> maxSumOfThreeSubarrays(vector<int>& nums, int k) {
        int len = nums.size(), overlap = 3;

        // sums for each subarrays    
        // Array content for example input:
        // 3 3 3 8 13 12 6 
        vector<int> sums(len - k + 1); 
        for (int i = 0, j = 0, sum = 0; i < nums.size(); i++) {
            sum += nums[i];
            if (i < k - 1) continue;
            sums[j] = sum; 
            sum -= nums[j++];            
        }

        // DP table for max sum of m non-overlap subarrays
        // Table content for example input:
        // 0  0  0  0  0  0  0 
        // 3  3  3  8  13 13 13 
        // 0  0  6  11 16 20 20 
        // 0  0  0  0  19 23 23 
        vector<vector<int>> max_sum(overlap + 1, vector<int>(len - k + 1));
        for (int i = 1, begin = 0; i < overlap + 1; i++, begin += 2) {
            int curr_max = 0, last = 0;
            for (int j = begin; j < len - k + 1; j++) {
                if (j >= k) last = max_sum[i - 1][j - k];
                max_sum[i][j] = curr_max = max(curr_max, last + sums[j]);
            }
        }

        // Retrieve starting indexes of subarrays with maximum sum from right-bottom of the DP table
        // Index:
        // (0)  1   2  (3)   4   (5)   6
        // Table:
        //  0   0   0   0    0    0    0  
        // (3)  3   3   8    13   13   13 
        //  0   0   6  (11)  16   20   20 
        //  0   0   0   0    19  (23)  23
        vector<int> res(overlap);
        int idx = len - k;
        for (int i = overlap; i > 0; i--) {
            while (idx > 0 && max_sum[i][idx] == max_sum[i][idx - 1])
                idx--;
            res[i - 1] = idx;
            idx -= k;
        }
     
        return res;
    }
};
```



