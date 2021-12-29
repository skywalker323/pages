# 327. Count of Range Sum

### Problem:

Given an integer array nums, return the number of range sums that lie in \[lower, upper\] inclusive.  
Range sum S\(i, j\) is defined as the sum of the elements in nums between indices i and j \(i ≤ j\), inclusive.

Note:  
A naive algorithm of O\(n2\) is trivial. You MUST do better than that.

Example:  
Given nums = \[-2, 5, -1\], lower = -2, upper = 2,  
Return 3.  
The three ranges are : \[0, 0\], \[2, 2\], \[0, 2\] and their respective sums are: -2, -1, 2.

### Solutions:

```cpp


struct Node {
        int val;
        long lo,hi;
        Node(int val, long lo, long hi): val(val), lo(lo), hi(hi) {}
    };
    void build(vector<Node>& st, vector<long>& pref, int node, int i, int j, long lo, long hi){
        if(i==j){
            st[node]=Node(0,lo,hi);
        }else{
            int mid=(i+j)/2;
            st[node]=Node(0,lo,hi);
            build(st,pref,2*node,i,mid,lo,pref[mid]);
            build(st,pref,2*node+1,mid+1,j,pref[mid+1],hi);
        }
    }
    void increment(vector<Node>& st, int node, int i, int j, long val){
        if(i==j){
            st[node].val++;
        }else{
            int mid=(i+j)/2;
            if(st[2*node].hi>=val){
                increment(st,2*node,i,mid,val);
            }else if(st[2*node+1].lo<=val){
                increment(st,2*node+1,mid+1,j,val);
            }
            st[node].val=st[2*node].val+st[2*node+1].val;
        }
    }
    int get(vector<Node>& st, int node, long lo, long hi){
        if(lo<=st[node].lo && st[node].hi<=hi){
            return st[node].val;
        }else if(st[node].hi < lo || hi < st[node].lo){
            return 0;
        }else{
            return get(st,2*node,lo,hi)+get(st,2*node+1,lo,hi);
        }
    }
    int countRangeSum(vector<int>& nums, int lower, int upper) {
        int n=nums.size();
        vector<long> pref(n+1,0);
        for(int i=0;i<n;i++){
            pref[i+1]=nums[i]+pref[i];
        }
        sort(pref.begin(),pref.end());
        vector<Node> st(4*(n+1), Node(0,0,0));
        build(st,pref,1,0,n,pref.front(),pref.back());
        int ans=0;
        long sum=0;
        for(int i=0;i<n;i++){
            increment(st,1,0,n,sum);
            sum+=nums[i];
            ans+=get(st,1,sum-upper,sum-lower);
        }
        return ans;
    }
    
    
// second approach
/*
So, the naive approach, which after some experience with medium problems, particularly like subarray sum etc. should come to one within a few minutes of thinking: Find the array of prefix sums, then use a nested loop to find all the sums which satisfy the given criteria. Of course O(n^2) will give TLE which is why this is HARD.

Since I am also a beginner, I thought about it for a while and then moved on to reading a solution to understand what was the next step.

I suppose for someone experienced, they may try some patterns with which n^2 problems are simplified, like DP or divide-and-conquer. The point of the hard problem is to start teaching you to inculcate this thinking of approaches when TLE after the naive solution is reached. So here, the mental question that should come to one's mind is,

For DP: If I know the solution to nums[start:i-1], can I calculate the solution to nums[start:i]?
For divide-and-conquer: If I know the solution to nums[start:mid] and nums[mid+1:end] (where mid = (start+end)/2 and end is the length of the array), can I calculate the solution to nums[start:end]?
In this case, it turns out that there is a divide and conquer solution. The solution is similar to merge sort.

Take the PREFIX SUM array (and not the original array). Let this be called sums.

If we have the solution to the left and right halves of the array, we can find the solution to the complete array by finding suitable pairs of prefix sums, one from the left half and the other from the right half, and adding these to the solution from the left and right halves of the array.

Now I will quote the crucial step from here: https://leetcode.com/problems/count-of-range-sum/discuss/1178174/Java-Clean-Merge-Sort-O(N-logN)-Solution-oror-with-detailed-Explanation

The merge sort based solution counts the answer while doing the merge. During the merge stage, we have already sorted the left half [start, mid) and right half [mid, end). We then iterate through the left half with index i. For each i, we need to find two indices k and j in the right half where

j is the first index satisfy sums[j] - sums[i] > upper and
k is the first index satisfy sums[k] - sums[i] >= lower.
Then the number of sums in [lower, upper] is j-k.

To understand this, consider any prefix sum after x elements. Consider another prefix sum after y elements such that x <= y. Then, if we know sums[x], then for x and y to form a range with a sum within the lower and upper bounds, then the conditions sums[y] - sums[x] >= lower and sums[y] - sums[x] <= upper, should be satisfied.

This gives the condition for y as sums[y] <= sums[x] + upper and sums[y] >= sums[x] + lower, and y >= x.

During merge sort note that the relative ordering between the left and right halves is maintained before the merging, so letting x belong to the left side of the array, y to the right half of the array maintains x <= y.

Hence if we make the count for each element in the left half of the array during the merge, then the count is guaranteed to be correct.

Also, due to sorted nature of subarrays used during merge, for a given x in the left subarray, since the right subarray is also sorted, it means that the elements within the desired range sums[x] + lower and sums[x] + upper are found in a contiguous chunk of the right subarray.

Moreover, since the left subarray is also sorted, sums[x] increases with x (monotonicity). This means that every time x is incremented, we can use the indices obtained for the range in the right subarray for the previous x, instead of starting both from 0, since both sums[x] + lower and sums[x] + upper can only increase.

In the quoted bullet points above, the updates ensure that at the end of the updating, the indices cover the required range for each index.

The base case here is that a single element will add to the count if the element value lies between lower and upper otherwise the solution will be zero.

To write the solution after understanding these steps, I used the following approach: Write merge sort in the usual way. Use two indices/pointers m and n starting from the beginning of the right subarray before the merge. At any block where the left subarray index is updated, update these two m and n. Add these to the total count. The function should return the sum of the counts of the left half, right half and the total count during the merge.

My first working solution in which I used the conventional merge sort template where following the main merge step there are two while loops, had to make the counts twice, once in the nested loop and once in the outside loop (since left subarray index is updated in these places). To get the form of the elegant solutions such as in the linked answer, I made the change to use an alternative form of merging where during every loop iteration, an update to the left index is guaranteed, and the merge concludes when the left subarray is completely traversed.
*/
class Solution {
public:
    
    int countWithMergeSort(vector<long> &sums, int left, int right, int lower, int upper)
    {
        int count = 0;
        
        if(right - left <= 1)
        {
            if(right - left == 1)
            {
                return (lower <= sums[left] && sums[left] <= upper);
            }
            else
            {
                return 0;
            }
        }
        
        int mid = (left + right)/2;
        
        int leftSideSum = countWithMergeSort(sums, left, mid, lower, upper);
        int rightSideSum = countWithMergeSort(sums, mid, right, lower, upper);
        
        
        int i = left;
        int j = mid;
        
        int n = 0;
        int m = 0;
        
        vector<long> cache(right - left, 0);
        
        int k = 0;
        
        
        while(i < mid)
        {
            

            while(mid+n < right && sums[mid+n] < sums[i]+lower)
                {
                    n++;
                }
            
            while(mid+m < right && sums[mid+m] <= sums[i] + upper)
                {
                    m++;
                }
            
            while(j < right && sums[j] < sums[i])
            {
                cache[k++] = sums[j++];
            }
            
            cache[k++] = sums[i++];
            
            count += m-n;
        }
        
        
        while(j < right)
        {
            cache[k++] = sums[j++];
        }
        
        
        for(int idx = 0; idx<cache.size(); idx++)
        {
            sums[left + idx] = cache[idx];
        }
    
        return leftSideSum + rightSideSum + count;
        
    }
    
    int countRangeSum(vector<int>& nums, int lower, int upper) {
        
        vector<long> prefixSum(nums.size(),0);
        
        int n = nums.size();
        
        prefixSum[0] = nums[0];
        
        for(int i = 1; i<nums.size(); i++)
        {
            prefixSum[i] = nums[i] + prefixSum[i-1];
        }
        
        return countWithMergeSort(prefixSum, 0, n, lower, upper);
    }
};
```



