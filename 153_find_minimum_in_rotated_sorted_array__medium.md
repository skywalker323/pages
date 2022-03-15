# 153 Find Minimum in Rotated Sorted Array – Medium

### Problem:

Suppose a sorted array is rotated at some pivot unknown to you beforehand.

\(i.e., 0 1 2 4 5 6 7 might become 4 5 6 7 0 1 2\).

Find the minimum element.

You may assume no duplicate exists in the array.

### Thoughts:

Using a O\(n\) iteration to find the min value in the array is too trivial and straight forward.  
So it is already clear enough that a Binary Search is needed here.

So this is a problem of modified Binary Search.

### Solutions:

```
class Solution {
public:
    int findMin(vector<int>& nums) {
        
        // Checking if the array is sorted or not      
        // If the array is already sorted ..just retuen the first element        
        if(nums[0]<nums[nums.size()-1]) return nums[0];
               
       // Find the Pivot element        
        int low=0;
        int high=nums.size()-1;        
        while(low<high)
        {
            int mid=low+(high-low)/2;
            if(nums[mid]>=nums[0])
                low=mid+1;
            else
                high=mid;         
        }
        return nums[high];
    }
};
```



