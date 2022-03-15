# 128 Longest Consecutive Sequence

### Problem:

Given an unsorted array of integers, find the length of the longest consecutive elements sequence.

For example,  
Given \[100, 4, 200, 1, 3, 2\],  
The longest consecutive elements sequence is \[1, 2, 3, 4\]. Return its length: 4.

Your algorithm should run in O\(n\) complexity.

### Solutions:

```java
public class Solution {
    public int longestConsecutive(int[] nums) {
        HashSet<Integer> data = new HashSet<Integer>();
        for (int i = 0; i < nums.length; i ++) {
            data.add(nums[i]);
        }
        int max = 0;
        for (int i = 0; i < nums.length; i ++) {
            if (data.contains(nums[i])) {
                int count = 1;
                int tmp = nums[i] - 1;
                while (data.contains(tmp)){
                    data.remove(tmp);
                    tmp --;
                    count ++;
                }
                tmp = nums[i] + 1;
                while (data.contains(tmp)) {
                    data.remove(tmp);
                    tmp ++;
                    count ++;
                }
                max = Math.max(max, count);
            }
        }
        return max;
    }
}
```



