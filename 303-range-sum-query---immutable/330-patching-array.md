# 330 Patching Array

### Problem:

Given a sorted positive integer array nums and an integer n, add/patch elements to the array such that any number in range \[1, n\] inclusive can be formed by the sum of some elements in the array. Return the minimum number of patches required.

Example 1:  
nums = \[1, 3\], n = 6  
Return 1.

Combinations of nums are \[1\], \[3\], \[1,3\], which form possible sums of: 1, 3, 4.  
Now if we add/patch 2 to nums, the combinations are: \[1\], \[2\], \[3\], \[1,3\], \[2,3\], \[1,2,3\].  
Possible sums are 1, 2, 3, 4, 5, 6, which now covers the range \[1, 6\].  
So we only need 1 patch.

Example 2:  
nums = \[1, 5, 10\], n = 20  
Return 2.  
The two patches can be \[2, 4\].

Example 3:  
nums = \[1, 2, 2\], n = 5  
Return 0.

### Solutions:

Memory exceeded solution

```java
/*
Every time count reaches a number that the next element in nums is greater than it, we need a patch.
If we add the number itself, count can be doubled because we can add the new number to any of the previous numbers.
So if count = 7, and the next number in nums is 10, if we add 7 to nums now we have all numbers until 14.
*/
class Solution {
public:
    int minPatches(vector<int>& nums, int n) {
        long patches = 0, count = 1, i = 0, sz = nums.size();
        while (count <= n) {
            
            if (i < sz && nums[i] <= count) count += nums[i++];
            
            else {
                count *= 2;
                patches++;
            }
        }
        
        return patches;
    }
};
```

Greedy working solution

```java
public class Solution {
    public int minPatches(int[] nums, int n) {
        long miss = 1;
        int count = 0;
        int i = 0;
        while (miss <= n) {
            if (i < nums.length && nums[i] <= miss) {
                miss += nums[i];
                i ++;
            }
            else {
                miss += miss;
                count ++;
            }
        }
        return count;
    }
}
```



