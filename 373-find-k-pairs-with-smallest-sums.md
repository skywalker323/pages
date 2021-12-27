# 373 Find K Pairs with Smallest Sums

### Problem:

You are given two integer arrays nums1 and nums2 sorted in ascending order and an integer k.

Define a pair \(u,v\) which consists of one element from the first array and one element from the second array.

Find the k pairs \(u1,v1\),\(u2,v2\) ...\(uk,vk\) with the smallest sums.

Example 1:

```
Given nums1 = [1,7,11], nums2 = [2,4,6],  k = 3

Return: [1,2],[1,4],[1,6]

The first 3 pairs are returned from the sequence:
[1,2],[1,4],[1,6],[7,2],[7,4],[11,2],[7,6],[11,4],[11,6]
```

Example 2:

```
Given nums1 = [1,1,2], nums2 = [1,2,3],  k = 2

Return: [1,1],[1,1]

The first 2 pairs are returned from the sequence:
[1,1],[1,1],[1,2],[2,1],[1,2],[2,2],[1,3],[1,3],[2,3]
```

Example 3:

```
Given nums1 = [1,2], nums2 = [3],  k = 3 

Return: [1,3],[2,3]

All possible pairs are returned from the sequence:
[1,3],[2,3]
```

### Solutions:

```cpp
Inspired by the index-array solution, we can further optimize the priority_queue solution where we only insert the next potential smallest one each time avoid inserting all the pairs at the first time. Time complexity now would be O(klogm) where k is from the problem while m is the size of the second array.

Analysis for validity If the current smallest pair is (i, j), then next potential smallest pair should be (i+1, j) or (i, j+1). But to ensure each pair will be covered, we can insert the next column index into the minimal heap only when the row index is 0. Minimal heap is where all the potential smallest pairs are stored. P.S. To ease the problem, you can put each index pair as each cell of a matrix composed of the index of the two arrays.

class Solution {
public:
    vector<pair<int, int>> kSmallestPairs(vector<int>& nums1, vector<int>& nums2, int k) 
    {
        vector<pair<int,int>> v;
        if(nums1.empty() || nums2.empty()) return v;
        auto cmp = [&nums1, &nums2](const pair<int, int>& a, const pair<int, int>&b) {
            return nums1[a.first]+nums2[a.second] > nums1[b.first]+nums2[b.second]; };
        priority_queue<pair<int, int>, vector<pair<int, int>>, decltype(cmp)> minHeap(cmp);
        minHeap.emplace(0, 0);
        while(minHeap.size() && k--)
        {
            auto t = minHeap.top(); minHeap.pop();
            v.emplace_back(nums1[t.first], nums2[t.second]);
            if(t.first<nums1.size()-1) minHeap.emplace(t.first+1, t.second);
            if(t.first==0 && t.second<nums2.size()-1) minHeap.emplace(t.first, t.second+1);
        }
        return v;
    }
};
```

```java
public class Solution {
    private class Pair implements Comparable<Pair>{
        private int i1;
        private int i2;
        private int sum;
        public Pair(int i1, int i2, int sum) {
            this.i1 = i1;
            this.i2 = i2;
            this.sum = sum;
        }
        public int compareTo(Pair p) {
            return this.sum - p.sum;
        }
    }
    public List<int[]> kSmallestPairs(int[] nums1, int[] nums2, int k) {
        int[] index = new int[nums1.length];
        List<int[]> result = new LinkedList<int[]>();
        if (nums1.length == 0 || nums2.length == 0) {
            return result;
        }
        PriorityQueue<Pair> q = new PriorityQueue<Pair>();
        for (int i = 0; i < nums1.length; i ++) {
            q.add(new Pair(i, 0, nums1[i] + nums2[0]));
            index[i] ++;
        }
        while (k > 0 && !q.isEmpty()) {
            Pair p = q.poll();
            result.add(new int[]{nums1[p.i1], nums2[p.i2]});
            k --;
            if (index[p.i1] < nums2.length) {
                q.add(new Pair(p.i1, index[p.i1], nums1[p.i1] + nums2[index[p.i1]]));
                index[p.i1] ++;
            }
        }
        return result;
    }
}
```



