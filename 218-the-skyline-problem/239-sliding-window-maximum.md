# 239 Sliding Window Maximum

### Problem:

Given an array nums, there is a sliding window of size k which is moving from the very left of the array to the very right. You can only see the k numbers in the window. Each time the sliding window moves right by one position.

For example,
Given nums = [1,3,-1,-3,5,3,6,7], and k = 3.
```
Window position                Max
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```
Therefore, return the max sliding window as [3,3,5,5,6,7].

Note:
You may assume k is always valid, ie: 1 ≤ k ≤ input array's size for non-empty array.

Follow up:
Could you solve it in linear time?

### Solutions:

```java
vector<int> maxSlidingWindow(vector<int>& nums, int k) {

        multiset<int> pq;
        vector<int> res;
        for(int i =0; i < k; i++)
            pq.insert(nums[i]);

        res.push_back(*pq.rbegin());

        for(int i=k; i < nums.size(); i++)
        {
            auto it = pq.find(nums[i-k]);
            pq.erase(it);
            pq.insert(nums[i]);
            res.push_back(*pq.rbegin());
        }

        return res;

    }
```
