# 56 Merge Intervals

### Problem:

Given a collection of intervals, merge all overlapping intervals.

For example,  
Given \[1,3\],\[2,6\],\[8,10\],\[15,18\],  
return \[1,6\],\[8,10\],\[15,18\].

### Solutions:

```java
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end());

        vector<vector<int>> merged;
        for (auto interval : intervals) {
            // if the list of merged intervals is empty or if the current
            // interval does not overlap with the previous, simply append it.
            if (merged.empty() || merged.back()[1] < interval[0]) {
                merged.push_back(interval);
            }
            // otherwise, there is overlap, so we merge the current and previous
            // intervals.
            else {
                merged.back()[1] = max(merged.back()[1], interval[1]);
            }
        }
        return merged;
    }
};
```



