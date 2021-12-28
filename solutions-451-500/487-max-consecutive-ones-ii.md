# 487 Max Consecutive Ones II

### Problem:

Given a binary array, find the maximum number of consecutive 1s in this array if you can flip at most one 0.

Example 1:

```
Input: [1,0,1,1,0]
Output: 4
Explanation: Flip the first zero will get the the maximum number of consecutive 1s.
    After flipping, the maximum number of consecutive 1s is 4.
```

Note:

1. The input array will only contain 0 and 1.
2. The length of input array is a positive integer and will not exceed 10,000

Follow up:  
What if the input numbers come in one by one as an infinite stream? In other words, you can't store all numbers coming from the stream as it's too large to hold in memory. Could you solve it efficiently?

### Solutions:

```java
public class Solution {
    public int findMaxConsecutiveOnes(int[] nums) {
        int[] count = new int[2];
        int index = 0;
        int max = 0;
        boolean zero = false;
        for (int i = 0; i < nums.length; i ++) {
            if (nums[i] == 1) {
                count[index] ++;
                max = Math.max(max, count[0] + count[1]);
            }
            else {
                zero = true;
                index = index ^ 1;
                count[index] = 0;
            }
        }
        if (zero == true) {
            max ++;
        }
        return max;
    }
}
```

```java
 int findMaxConsecutiveOnes(vector<int>& nums) {
        // For each 0, we need to figure out # of 1s before and after. Going through the nums array, skip all
        // consecutive 1s until we reach a 0, then check if this 0 can be ignored or not (aka if we already flip a 0),
		// and update the result
        int res = 0, prevZeroInd = -1, ones = 0, zeros = 0; // result, index of previous 0, consecutive 1s, and # of flipped 0s
        for (int i = 0; i < nums.size(); ++i)
        {
            ones = nums[i] == 1 || zeros == 0 ? ones + 1 : i - prevZeroInd; // update consecutive 1s
            zeros = nums[i] == 0 && zeros == 0 ? 1 : zeros; // check if we can flip the zero
            prevZeroInd = nums[i] == 0 ? i : prevZeroInd; // update index of previous 0
            res = max(res, ones); // update the result
        }
        
        return res;
    }
```



