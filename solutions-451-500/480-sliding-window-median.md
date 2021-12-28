# 480 Sliding Window Median

### Problem:

Median is the middle value in an ordered integer list. If the size of the list is even, there is no middle value. So the median is the mean of the two middle value.

Examples:   
\[2,3,4\] , the median is 3

\[2,3\], the median is \(2 + 3\) / 2 = 2.5

Given an array nums, there is a sliding window of size k which is moving from the very left of the array to the very right. You can only see the k numbers in the window. Each time the sliding window moves right by one position. Your job is to output the median array for each window in the original array.

For example,  
Given nums = \[1,3,-1,-3,5,3,6,7\], and k = 3.

```
Window position                Median
---------------               -----
[1  3  -1] -3  5  3  6  7       1
 1 [3  -1  -3] 5  3  6  7       -1
 1  3 [-1  -3  5] 3  6  7       -1
 1  3  -1 [-3  5  3] 6  7       3
 1  3  -1  -3 [5  3  6] 7       5
 1  3  -1  -3  5 [3  6  7]      6
```

Therefore, return the median sliding window as \[1,-1,-1,3,5,6\].

Note:   
You may assume k is always valid, ie: 1 ≤ k ≤ input array's size for non-empty array.

### Solutions:

```java
The reason to use two heaps is to have O(1) lookup for the median. It's is O(n) if we use multiset, but what 
if we reuse the median pointer when we can?
The solution below still has the O(n^2) worst case run-time, and the average case run-time is O(n * log n).
We can achieve O(n * log n) in the worst case if we make sure that the multiset comparator is stable.

vector<double> medianSlidingWindow(vector<int>& nums, int k)
{
    int size = nums.size(), median_pos = k - k / 2 - 1;
    vector<double> res(size - k + 1);
    multiset<int> s(nums.begin(), nums.begin() + k);
    auto it = next(s.begin(), median_pos);

    for (auto i = k; i <= size; ++i)
    {
        res[i - k] = ((double)*it + (k % 2 != 0 ? *it : *next(it))) / 2;
        if (i < size)
        {
            // magic numbers (instead of enum) for brevity. INT_MAX means to retrace the iterator from the beginning.
            int repos_it = INT_MAX; 
            if (k > 2)
            {
                // if inserted or removed item equals to the current median, we need to retrace.
                // we do not know which exact element will be removed/inserted, and we cannot compare multiset iterators.
                // otherwise, we can keep or increment/decrement the current median iterator.
                if ((nums[i - k] < *it && nums[i] < *it) || (nums[i - k] > *it && nums[i] > *it)) repos_it = 0;
                else if (nums[i - k] < *it && nums[i] > *it) repos_it = 1; // advance forward.
                else if (nums[i - k] > *it && nums[i] < *it) repos_it = -1; // advance backward.
            }
            s.insert(nums[i]);
            s.erase(s.find(nums[i - k]));

            if (repos_it == INT_MAX) it = next(s.begin(), median_pos);
            else if (repos_it == 1) ++it;
            else if (repos_it == -1) --it;
        }
    }
    return res;
}
```



