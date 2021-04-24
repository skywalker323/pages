# 280. Wiggle Sort

### Problem:

Given an unsorted array nums, reorder it in-place such that nums[0] <= nums[1] >= nums[2] <= nums[3]....

For example, given nums = [3, 5, 2, 1, 6, 4], one possible answer is [1, 6, 2, 5, 3, 4].

### Solutions:

```cpp
class Solution {
public:
    void wiggleSort(vector<int>& nums) {
        if(nums.empty() ) return;
        sort(nums.begin(), nums.end());
        for(int i =1; i < nums.size() -1; i=i+2)
        {
            swap(nums[i], nums[i+1]);
        }
    }
};
```

```java
public class Solution {
   public void wiggleSort(int[] nums) {
    boolean less = true;
    for (int i = 0; i < nums.length - 1; i++) {
        if (less) {
            if (nums[i] > nums[i + 1]) {
                swap(nums, i, i + 1);
            }
        } else {
            if (nums[i] < nums[i + 1]) {
                swap(nums, i, i + 1);
            }
        }
        less = !less;
    }
}
}
```
