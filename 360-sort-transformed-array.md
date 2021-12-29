# 360. Sort Transformed Array

### Problem:

Given a sorted array of integers nums and integer values a, b and c. Apply a function of the form f\(x\) = ax^2 + bx + c to each element x in the array.

The returned array must be in sorted order.

Expected time complexity: O\(n\)

Example:  
nums = \[-4, -2, 2, 4\], a = 1, b = 3, c = 5,

Result: \[3, 9, 15, 33\]

nums = \[-4, -2, 2, 4\], a = -1, b = 3, c = 5

Result: \[-23, -5, 1, 7\]

### Solutions:

```java
// End results be sorted by the distance to the axis of symmetry of the quadratic function.

class Solution {
public:
    vector<int> sortTransformedArray(vector<int>& nums, int a, int b, int c) {
        std::vector<int> res(nums.size(), 0);
        double d0 = static_cast<double>(-b) / (2.0 * a);
        int l = 0;
        int r = nums.size() - 1;
        int idx = 0;
        int x = 0;
        
        while (l <= r) {
            if (std::abs(d0 - nums[l]) < std::abs(d0 - nums[r])) {
                x = nums[r];
                --r;
            } else {
                x = nums[l];
                ++l;
            }
            res[idx++] = a * x * x + b * x + c;
        }
		// Depending on the sign of a. The results will be sorted in as/dsending order.
        if (res[0] > res.back()) {
            std::reverse(res.begin(), res.end());
        }
        return res;
        
    }
};

```



