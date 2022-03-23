# 33 LeetCode Java: Search in Rotated Sorted Array

### Problem:

Suppose a sorted array is rotated at some pivot unknown to you beforehand.

\(i.e., 0 1 2 4 5 6 7 might become 4 5 6 7 0 1 2\).

You are given a target value to search. If found in the array return its index, otherwise return -1.

You may assume no duplicate exists in the array.

---

### Thoughts

If the sorted array is not rotated, then this is a question of standard binary search. However, when the array is rotated, we will need some modification of standard binary search.

Modify the checking condition when moving start and end pivot in standard binary search. We get the mid element of given start and end. If the element equals the target, then good, we can return the index.

After we calculate the mid index, let’s see where the mid is at. In the picture above, there are two cases for the mid point. Either at A or at B.  
When mid is at A, we need to find a way to determine if we want to go to the left half or right half. We can see that the easier way is to check the left half, which is nums\[start\] &lt;=target &lt; nums\[mid\]. Otherwise, it's falling into the right half.  
Similar strategy applies for the case when mid is at point B.

---

### Solutions:

```java
int search(vector<int>& nums, int target) {
        int l = 0, h = (int)nums.size() - 1;
        while (l <= h) {
            int m = (l + h) / 2;
            if (target == nums[m])
                return m;
            
            // left side is sorted
            if (nums[m] >= nums[l])
            {   
                // number exists in this sorted range
                if (nums[l] <= target && nums[m] > target) 
                    h = m - 1;
                else
                    l = m + 1;
            }
            else
            {
                assert(nums[m] <= nums[h]); // right side is sorted
                
                // number exists in this sorted range
                if (nums[m] < target && nums[h] >= target)
                    l = m + 1;
                else
                    h = m - 1;
            }
        }
        return - 1;
    }
```



