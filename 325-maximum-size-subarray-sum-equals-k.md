# 325 Maximum Size Subarray Sum Equals k

### Problem:

Given an array nums and a target value k, find the maximum length of a subarray that sums to k. If there isn't one, return 0 instead.

Note:  
The sum of the entire nums array is guaranteed to fit within the 32-bit signed integer range.

Example 1:  
Given nums = \[1, -1, 5, -2, 3\], k = 3,  
return 4. \(because the subarray \[1, -1, 5, -2\] sums to 3 and is the longest\)

Example 2:  
Given nums = \[-2, -1, 2, 1\], k = 1,  
return 2. \(because the subarray \[-1, 2\] sums to 1 and is the longest\)

Follow Up:  
Can you do it in O\(n\) time?

### Solutions:

```java
public class Solution {
    public int maxSubArrayLen(int[] nums, int k) {
        int max = 0;
        for (int i = 0; i < nums.length; i ++) {
            int sum = 0;
            for (int j = i; j < nums.length; j ++) {
                sum += nums[j];
                if (sum == k) {
                    max = Math.max(max, j - i + 1);
                }
            }
        }
        return max;
    }
}
```

```java
class Solution {
public:
    int maxSubArrayLen(vector<int>& nums, int k) {
        int cur = 0;
        unordered_map<int, int> mp;
        mp[0] = -1;
        int result = INT_MIN;
        for (int i = 0; i < nums.size(); ++i) {
            cur += nums[i];
            if (mp.find(cur - k) != mp.end()) {
                result = max(result, i - mp[cur - k]);
            }
            //maintaining the farthest possible index for each subarray
            if (mp.find(cur) == mp.end()) mp[cur] = i;
        }
        return (result == INT_MIN? 0: result);
    }
};
```



