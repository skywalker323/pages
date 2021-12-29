# 324. Wiggle Sort II

### Problem:

Given an unsorted array nums, reorder it such that nums\[0\] &lt; nums\[1\] &gt; nums\[2\] &lt; nums\[3\]....

Example:  
\(1\) Given nums = \[1, 5, 1, 1, 6, 4\], one possible answer is \[1, 4, 1, 5, 1, 6\].   
\(2\) Given nums = \[1, 3, 2, 2, 3, 1\], one possible answer is \[2, 3, 1, 3, 1, 2\].

Note:  
You may assume all input has valid answer.

Follow Up:  
Can you do it in O\(n\) time and/or in-place with O\(1\) extra space?

### Solutions:

```java
public class Solution {
    public void wiggleSort(int[] nums) {
        int[] tmp = new int[nums.length];
        Arrays.sort(nums);
        int left = (nums.length - 1) / 2, right = nums.length - 1;
        for (int i = 0; i < tmp.length; i ++) {
            if (i % 2 == 0) {
                tmp[i] = nums[left--];
            }
            else {
                tmp[i] = nums[right--];
            }
        }
        for (int i = 0; i < tmp.length; i ++) {
            nums[i] = tmp[i];
        }
    }
}
```

```java
/*A solution that doesn't need to calculate the index:

Use the nth_element that comes with c++ to divide the entire array by the median, and take the median as pivot

The three pointers are: the loop pointer i, which is less than the median pointer e, because the number less than the median must be at an even index, named e, which means even, starting from the last even index. Greater than the median pointer o, because the number greater than the median must be at an odd index, named o, which means odd, starting from 1.

By traversing the entire array i, when it is found that the element i is greater than pivot, there are two cases. First: i is an even number, but the value greater than pivot should be at an odd index, so exchange the value of i and the current o, and add two to o and move to The next odd index. This is to ensure that o and the odd index before it are both greater than the median.

Second: We found that i has surpassed o and forced the exchange to ensure that o and its previous odd index are both greater than the median.

The judgment of e is similar, but from back to front.
*/

void wiggleSort(vector<int> &nums) {
        // write your code here
        int m = (nums.size() -1)/2;
        nth_element(nums.begin(), nums.begin()+m, nums.end());
        int pivot = nums[m];
        
        int i = 0;
        int o = 1;
        int e = (nums.size()-1) % 2 == 0 ? nums.size()-1 : nums.size()-2;
        //cout << m;
        while (i < nums.size()) {
            if (nums[i] > pivot && (i % 2 == 0 || i > o)) {
                swap(nums[i], nums[o]);
                o += 2;
                continue;
            }
            if (nums[i] < pivot && (i % 2 == 1 || i < e)) {
                swap(nums[i], nums[e]);
                e -= 2;
                continue;
            }
            i++;
        }
    }
```



