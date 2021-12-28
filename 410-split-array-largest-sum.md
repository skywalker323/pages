# 410 Split Array Largest Sum

### Problem

Given an array which consists of non-negative integers and an integer m, you can split the array into m non-empty continuous subarrays. Write an algorithm to minimize the largest sum among these m subarrays.

Note:  
If n is the length of array, assume the following constraints are satisfied:

1 ≤ n ≤ 1000  
1 ≤ m ≤ min\(50, n\)  
Examples:

```
Input:
nums = [7,2,5,10,8]
m = 2

Output:
18

Explanation:
There are four ways to split nums into two subarrays.
The best way is to split it into [7,2,5] and [10,8],
where the largest sum among the two subarrays is only 18.
```

### Solutions

```cpp
First thing first, below is the code:

class Solution {
private:
    bool doable (const vector<int>& nums, int cuts, long long max) {
        int acc = 0;
        for (num : nums) {
            // This step is unnecessary for this problem. I didn't discard this line because I want doable function more generalized.
            if (num > max) return false;
            else if (acc + num <= max) acc += num;
            else {
                --cuts;
                acc = num;
                if (cuts < 0) return false;
            }
        }
        return true;
    }

public:
    int splitArray(vector<int>& nums, int m) {
        long long left = 0, right = 0;
        for (num : nums) {
            left = max(left, (long long)num);
            right += num;
        }

        while (left < right) {
            long long mid = left + (right - left) / 2;
            if (doable(nums, m - 1, mid)) right = mid;
            else left = mid + 1;
        }
        return left;
    }
};
Introduction to this problem:
We can break this problem into two smaller problems:

Given an array (A), number of cuts (CUTS), and the Largest sum of sub-arrays (MAX). 
Can you use at most CUTS cuts to segment array A into CUTS + 1 sub-arrays, such that the sum of each sub-array 
is smaller or equal to MAX?
Given a lower bound (left), an upper bound (right), an unknown bool array (B), and an API uses i as input and 
tells you whether B[i] is true. If we know there exists an index k, that B[i] is false when i < k, and B[i] is
 true when i >= k. What is the fastest way to find this k (the lower bound)?
Solution to the first sub-problem (Skip this part if you already knew how to solve 1st sub-problem):
For the first question, we can follow these steps:

For each element in the array, if its value is larger than MAX, we know it's not possible to cut this array
into groups that the sum of all groups are smaller than MAX. (Reason is straightforward, if A is [10, 2, 3, 5] and
MAX is 6, even you have 3 cuts by which you can cut A as [[10], [2], [3], [5]], the group containing 10 will still
be larger than 6). Use greedy algorithm to cut A. Use an accumulator ACC to store the sum of the currently processed
group, and process elements in A one by one. For each element num, if we add num with ACC and the new sum is still 
no larger than MAX, we update ACC to ACC + num, which means we can merge num into the current group. If not, we must
use a cut before num to segment this array, then num will be the first element in the new group.
If we didn't go through A but already used up all cuts, then it's not possible only using CUTS cuts to segment this 
array into groups to make sure sum of each sub-array is smaller than MAX. Otherwise, if we can reach the end of A 
with cuts left (or use exactly CUTS cuts). It's possible to do so. Then the first question is solved.

Solution to the second sub-problem(Skip this part if you already knew how to solve 2nd sub-problem):
The array B will be something like [false, false, ..., false, true, true, ..., true]. We want to find the index of 
the first true. Use binary search to find this k. Keep a value mid, mid = (left + right) / 2. If B[mid] = false, then
move the search range to the upper half of the original search range, a.k.a left = mid + 1, otherwise move search range
to the lower half, a.k.a right = mid.
Why this algorithm is correct...
No matter how we cut the array A, the Largest sum of sub-arrays will fall into a range [left, right]. Left is the
 value of the largest element in this array. right is the sum of this array. (e.g., Given array [1, 2, 3, 4, 5], 
 if we have 4 cuts and cut it as [[1], [2], [3], [4], [5]], the Largest sum of sub-arrays is 5, it cannot be smaller.
  And if we have 0 cut, and the only sub-array is [[1, 2, 3, 4, 5]], the Largest sum of sub-arrays is 15, it cannot be
   larger).
However, we cannot decide the number of cuts (CUTS), this is an given constraint. But we know there must be a magic 
number k, which is the smallest value of the Largest sum of sub-arrays when given CUTS cuts. When the Largest sum of
 sub-arrays is larger than k, we can always find a way to cut A within CUTS cuts. When the Largest sum of sub-arrays
  is smaller than k, there is no way to do this.
Example
For example, given array A [1, 2, 3, 4, 5]. We can use 2 cuts.

No matter how many cuts are allowed, the range of the possible value of the Largest sum of sub-arrays is [5, 15].
When given 2 cuts, we can tell the magic number k here is 6, the result of segmentation is [[1, 2, 3], [4], [5]].
When Largest sum of sub-arrays is in range [6, 15], we can always find a way to cut this array within two cuts. 
You can have a try.
However, when Largest sum of sub-arrays is in range [5, 5], there is no way to do this.
This mapped this problem into the second sub-problem. Bool array B here is [5:false, 6:true, 7:true, 8:true, ..., 
15:true]. We want to find the index i of the first true in B, which is the answer of this entire question, and by
 solving the first sub-problem, we have an API that can tell us given an i (Largest sum of sub-arrays), whether B[i] is
  true (whether we can find a way to cut A to satisfy the constraint).
Below is the code with comment, just in case you don't have time to read the explanations above.

class Solution {
private:
    /* 
        Params:
            nums - The input array; 
            cuts - How many cuts are available (cuts = #groups - 1); 
            max - The maximum of the (sum of elements in one group);
        Rtn:
            Whether we can use at most 'cuts' number of cuts to segment the entire array, 
            such that the sum of each group will not exceed 'max'.
     */
    bool doable (const vector<int>& nums, int cuts, long long max) {

        // 'acc' is the temporary accumulator for the currently processed group.

        int acc = 0;
        for (num : nums) {

// If the current processed element in this array is larger than 'max', we cannot segment the array.
// (Reason is straightforward, if 'nums' is [10, 2, 3, 5] and 'max' is 6, even you can have 3 cuts
// (by which you can cut array as [[10], [2], [3], [5]]), the group containing 10 will be larger than 6, 
//  there is no way to do this).
// Ps: This step is unnecessary in this solution. Because 'left' in the splitArray() function can assure 
// 'max' will be larger than every single element. I just want to write a generalized doable() function :)

            if (num > max) return false;

// If the (sum of the currently processed group) + (current element) is smaller than max, we can add current 
// element into this group.

            else if (acc + num <= max) acc += num;

// If not, we will make a cut before this element, and this element will be the first element in the new group.
            else {
                --cuts;
                acc = num;

// If we've used up all cuts, this means this 'max' is not doable.
                if (cuts < 0) return false;
            }
        }

// If we can reach here, this means we've used at most 'cuts' cut to segment the array, and the sum of each groups is
// not larger than 'max'. Yeah!
        return true;
    }

public:
    int splitArray(vector<int>& nums, int m) {
// Use long long to avoid overflow.
        long long left = 0, right = 0;
// The smallest possible value ('left') is the the value of the largest element in this array.
// The largest possible value ('right') is the sum of all elements in this array.
        for (num : nums) {
            left = max(left, (long long)num);
            right += num;
        }

// Use binary search, find the lower bound of the possible (minimum sum of groups within m - 1 cuts).
        while (left < right) {
            long long mid = left + (right - left) / 2;
            if (doable(nums, m - 1, mid)) right = mid;
            else left = mid + 1;
        }
        return left;
    }
};
```



