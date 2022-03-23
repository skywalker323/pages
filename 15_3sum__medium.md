# 15 3Sum – Medium

### Problem:

Given an array S of n integers, are there elements a, b, c in S such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero.

Note:

Elements in a triplet \(a,b,c\) must be in non-descending order. \(ie, a ≤ b ≤ c\)  
The solution set must not contain duplicate triplets.  
    For example, given array S = {-1 0 1 2 -1 -4},

```
A solution set is:
(-1, 0, 1)
(-1, -1, 2)
```

### Thoughts:

Key idea here is to sort the array first. Then start from left to pick the first element to be a, then find if there are two other elements b, c make a + b + c = 0. Another thing to keep in mind is that how to avoid duplicate answers.

### Solutions:

```java
vector<vector<int>> threeSum(vector<int>& nums) {
        sort(begin(nums), end(nums));
         vector<vector<int>>   res;
        for(int i =0; i < nums.size(); i++)
        {
            if( i==0 || nums[i-1] != nums[i])
            {
                int n1 = nums[i];
                int start = i+1;
                int end = nums.size()-1;

                while( start < end)
                {
                    auto sum = n1 + nums[start] + nums[end];
                    if(  sum == 0)
                    {
                        res.push_back({n1, nums[start], nums[end]});
                        auto sv = nums[start];
                        while(start < end and sv == nums[start]) start++;
                        auto ev = nums[end];
                        while(start < end and ev == nums[end]) end--;

                    }
                    else if( sum < 0 )
                    {
                        start++;
                    }
                    else
                    {
                        end--;
                    }
                }
            }
        }
        
        return res;
    }
```



