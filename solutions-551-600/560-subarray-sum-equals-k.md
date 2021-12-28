# 560. Subarray Sum Equals K

### Problem:

Given an array of integers and an integer k, you need to find the total number of continuous subarrays whose sum equals to k.

Example 1:

```
Input:nums = [1,1,1], k = 2
Output: 2
```

Note:  
1. The length of the array is in range \[1, 20,000\].  
2. The range of numbers in the array is \[-1000, 1000\] and the range of the integer k is \[-1e7, 1e7\].

### Solutions:

```java

You may come up with two pointers/ sliding window technique at the very begin thought, which is not right after you 
have tried. Here is the general summary to expalin why we can not apply two pointers to solve this problem.

Let go through the process of explanation by introducing what kind of problem can be solved with two pointers, 
such that if this problem doesn't match the criteria, it cannot be solved with two pointers.

A problem can be solved by two pointers when two pointers come into place to help us reduce the total cases we need
 to consider, such that the corresponding time complexity will reduce too.

Let's trying to solve longest-substring-without-repeating-characters firstly, which is a problem that can be solved
 by two pointers. After that, I will generalize the characteristics of that problem, such that we can know what
  kind of problem can be solved by two pointers.

Given a string, find the length of the longest substring without repeating characters.

Example 1:

Input: "abcabcbb"
Output: 3 
Explanation: The answer is "abc", with the length of 3. 
The very begin thought to solve problem above will be applying a brute-force solution, in which we maintain two index, 
i and j, such that for every i we test whether any possible j that form a substring s.substring(i,j)(both inclusive) 
doesn't contains repeating characters. This cost O(n^3).

Now we need to find a better solution to reduce time complexity. The brute-force solution is not efficient, 
because it tests some cases that doesn't need to consider.

case that are never valid: e.g., forInput: "abcabcbb", and i == 0, j ==3 such that we are testing substringabca 
right now. we found a repeating character a when seeing s.charAt(j) =='a', so we know that for a certain i == 0, 
the rightmost j will be j == 2 , because when j == 3 it contains repeating characters(s.charAt(3) =='a'), and
 when j == 4 it contains repeating characters(s.charAt(3) =='a') and when j == 5 it contains repeating
  characters(s.charAt(3) =='a'), ... and when j == n - 1 it still contains repeating characters(s.charAt(3) =='a').
After oberserving that, we no long need to test every j such thatj >= 3 when i == 0, so we can increase i by 1 .

case that are included in the result already:
Wait, after i = i + 1, what about j? Should j move left or move right or not move? Let go back to brute-force solution 
to get some intuition. In brute-force solution, whenever i++, the j should be j = i + 1. However if we apply
 j = i + 1, we still test some cases that should not being tested because they must be valid and they are included 
 in the result alreay. For isntance, in case i == 0 and j == 3, we do i = i + 1 such that i becomes 1 now. 
 we do not need to test case where i == 1 and j == 2 becuase we have tested case where i == 0 and j == 2, and 
 it have been included in the result already. i == 1 and j == 2 is a sub case of i == 0 and j == 2, since 
 result of i == 0 and j == 2 have been put in result, we no longer need to test i == 1 and j == 2. Consider 
 if I tell you that I have 3 cats, will you doubt that I have at least 2 cats?

So the result is we will make i++ and j stay at the same index before next round. (though in next round j++)

Above is the specific problem that can be solved by two pinters. Now let's generalize the characteristics of
 the problems that can be solved by two pinters. The summary is simple:

If a wider scope of the sliding window is valid, the narrower scope of that wider scope is valid mush hold.
If a narrower scope of the sliding window is invalid, the wider scope of that narrower scope is invalid mush hold.
With 2 rules above hold, we are able to optimize the brute-force solution to two pointers solution.

I just show you what kind of question can be solved by two pointers by using some very simple Induction Reasoning.
 Now let me show you why this problem cannot be solved by two pointers. Like I said, If this problem doesn't
  meet the creteria that two pointer technique, it cannot be solved with two pointers.

In this specific problem, rule 1 doesn't hold, because I can find a specific case such that it doesn't hold, e.g.,
 if K is 3, 1,1,1 sum is 3, so 1,1,1, is valid, but 1,1 sum is 2 which means 1,1 is invalid, so rule 1 breaks.

Rule2 doesn't hold, either, because I can find a specific case such that it doesn't hold, e.g., if K is 2, 1,1,1
 sum is 3, so 1,1,1, is invalid, but 1,1,1,-1 sum is 2 which means 1,1,1,-1 is valid, so rule 2 breaks.



Solution1 (TLE)
Basic brute-force approach - for all sum[i,j] count how many times we saw k. O(n^3).

public int subarraySum(int[] nums, int k) {
        int count = 0;
        for(int x=0; x < nums.length; x++){
            for(int y=x; y < nums.length; y++){
                int sum = 0;
                for(int z=x; z <= y; z++)
                    sum += nums[z];
                if(sum == k)
                    ++count;
            }
        }
        return count;
    }
Solution2 - make use of prefix sum for the third for-loop above. O(n^2)

public int subarraySum(int[] nums, int k) {
        int count = 0;
        for(int x=1; x < nums.length; x++)
            nums[x] += nums[x-1];
        for(int x=0; x < nums.length; x++){
            if(nums[x] == k)
                ++count;
            for(int y=x+1; y < nums.length; y++)
                if(nums[y]-nums[x] == k)
                    ++count;
        }
        return count;
    }
Solution3 - Remember the frequency of all prefix sums. O(n) time and O(n) memory.
sum to keep track of sum of all the elements so far. If we can find a prefix sum in the map for sum-k, it means we can form sum == k using the elements after the element corresponding to that prefix sum till the current element (included). Count all such sums at each element.

There is a special case like in the Solution2 when nums[x] == k that is current sum itself is equal to target without any subtractions. For this solution, we can either increment count by 1 whenever sum == k below or make an entry as a special case in our map
preSumFreq.put(0, 1) to cover those cases.

public int subarraySum(int[] nums, int k) {
        int count = 0, sum=0;
        Map<Integer, Integer> preSumFreq = new HashMap<>();
        preSumFreq.put(0, 1);
        for(int i=0; i < nums.length; i++){
            sum += nums[i];
            count += preSumFreq.getOrDefault(sum-k, 0);
            preSumFreq.put(sum, preSumFreq.getOrDefault(sum,0)+1);
        }
        return count;
    }


```





