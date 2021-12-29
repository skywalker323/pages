# 378 Kth Smallest Element in a Sorted Matrix

Given a n x n matrix where each of the rows and columns are sorted in ascending order, find the kth smallest element in the matrix.

Note that it is the kth smallest element in the sorted order, not the kth distinct element.

Example:

```
matrix = [
   [ 1,  5,  9],
   [10, 11, 13],
   [12, 13, 15]
],
k = 8,

return 13.
```

Note:   
You may assume k is always valid, 1 ≤ k ≤ n^2.

### Solutions:

```java
class Solution {
public:
    int kthSmallest(vector<vector<int>>& matrix, int k) {
        priority_queue<int> heap;
        
        for (auto const &row : matrix) {
            for (auto val : row) {
                if (heap.size() < k || val <= heap.top()) {
                    heap.push(val);
                }
                if (heap.size() > k) {
                    heap.pop();
                }
            }
        }
        
        return heap.top();
    }
};
```

```java
public class Solution {
    public int kthSmallest(int[][] matrix, int k) {
        int n=matrix.length;
        int lower = matrix[0][0];
        int upper = matrix[n-1][n-1];
        while(lower < upper){
            int mid = lower + (upper-lower) / 2;
            int count = count(matrix, mid);
            if(count<k){
                lower = mid+1;
            }else{
                upper = mid;
            }
        }

        return upper;
    }

    private int count(int[][] matrix, int target){
        int n = matrix.length;
        int i = m-1;
        int j = 0;
        int count = 0;

        while(i >= 0 && j < n){
            if(matrix[i][j] <= target){
                count += i+1;
                j++;
            }else{
                i--;
            }
        }
        return count;
    }
}
```



