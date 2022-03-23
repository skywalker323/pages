# 4. Median of Two Sorted Arrays

### Problem:

There are two sorted arrays nums1 and nums2 of size m and n respectively.

Find the median of the two sorted arrays. The overall run time complexity should be O\(log \(m+n\)\).

Example 1:

```
nums1 = [1, 3]
nums2 = [2]

The median is 2.0
```

Example 2:

```
nums1 = [1, 2]
nums2 = [3, 4]

The median is (2 + 3)/2 = 2.5
```

### Solutions:

```java
double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2)
     {
         int n1 = nums1.size();
         int n2 = nums2.size();
         int len = n1 + n2;

         if (n1 > n2) return findMedianSortedArrays(nums2, nums1);
         if (n1 == 0) return (nums2[(n2 - 1) / 2] + nums2[n2 / 2]) / 2.0;

         int left = 0, right = n1;
         // do a binary search in arr1 such that no of elems in left partion of ar1 and left ar2 is equal to len/2
         // partition conditions : max(left part of n1) < max(right part of n2) and
         // max(left part of n2) <  max(right part of n1) then partition done
         // otherwise find repeat the process 
         while (left <= right)
         {
             int cut1 = left + (right - left) / 2;
             int cut2 = len / 2 - cut1;

             int L1 = (cut1 == 0) ? INT_MIN : nums1[cut1 - 1];
             int L2 = (cut2 == 0) ? INT_MIN : nums2[cut2 - 1];
             int R1 = (cut1 == n1) ? INT_MAX : nums1[cut1];
             int R2 = (cut2 == n2) ? INT_MAX : nums2[cut2];
             // l1 < r2 or l2 < r1 then partition done
             if (L1 > R2) right = cut1;
             else if (L2 > R1) left = cut1 + 1;
             else
             {
                 // parition conds met so find the median (median lies in l1, l2, r1, r2)
                 // visualize how much sort will arrange these elems if they are merging and find median
                 if (len % 2 == 0) return (max(L1, L2) + min(R1, R2)) / 2.0;
                 return min(R1, R2);

             }
         }
```



