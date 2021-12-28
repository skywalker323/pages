# 448 Find All Numbers Disappeared in an Array

### Problem:

Given an array of integers where 1 ≤ a\[i\] ≤ n \(n = size of array\), some elements appear twice and others appear once.

Find all the elements of \[1, n\] inclusive that do not appear in this array.

Could you do it without extra space and in O\(n\) runtime? You may assume the returned list does not count as extra space.

Example:

```
Input:
[4,3,2,7,8,2,3,1]

Output:
[5,6]
```

### Solutions:

```java
public class Solution {
    public List<Integer> findDisappearedNumbers(int[] nums) {
        int i = 0;
        while(i < nums.length) {
            if (nums[i] == i + 1) {
                i ++;
                continue;
            }
            int index = nums[i] - 1;
            if (nums[index] == index + 1) {
                i ++;
                continue;
            }
            nums[i] = nums[index];
            nums[index] = index + 1;
        }
        List<Integer> result = new LinkedList<Integer>();
        for (int j = 0; j < nums.length; j ++) {
            if (nums[j] != j + 1) {
                result.add(j + 1);
            }
        }
        return result;
    }
}
```

```java
1st for loop: for each value x, negate the element at xth position
2nd for loop: get all the positions that has a positive element. These are the missing values to return.

    vector<int> findDisappearedNumbers(vector<int>& nums) {
        vector<int> ans;

        // 1st for loop: nums = [4,3,2,7,8,2,3,1]
        for(int i = 0; i < nums.size(); i++)  // each iteration:
        {                                    
// i = 0              i = 1               i = 2                ... i = 7
            int temp = nums[i];               
// temp = 4           temp = 3            temp = -2            ... temp = -1
            temp = (temp > 0) ? temp : -temp; 
// temp = 4           temp = 3            temp = 2             ... temp = 1
            if(nums[temp-1] > 0)              
// nums[3] > 0        nums[2] > 0         nums[1] > 0          ... nums[0] > 0
                nums[temp-1] *= -1;           
// [4,3,2,-7,8,2,3,1] [4,3,-2,-7,8,2,3,1] [4,-3,-2,-7,8,2,3,1] ... [-4,-3,-2,-7,8,2,-3,-1]
        } 

// 2nd for loop: nums = [-4,-3,-2,-7,8,2,-3,-1]
        for(int i = 0; i < nums.size(); i++)
            if(nums[i] > 0)         
// the 4th & 5th indexes are positive
                ans.push_back(i+1); 
// ans = [5,6]

        return ans;
    }
```



