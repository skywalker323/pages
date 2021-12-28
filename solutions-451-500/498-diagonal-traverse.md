# 498 Diagonal Traverse

### Problem

Given a matrix of M x N elements \(M rows, N columns\), return all elements of the matrix in diagonal order as shown in the below image.

Example:

```
Input:
[
 [ 1, 2, 3 ],
 [ 4, 5, 6 ],
 [ 7, 8, 9 ]
]
Output:  [1,2,4,7,5,3,6,8,9]
Explanation:
```

![](/assets/diagonal_traverse.png)

Note:  
1. The total number of elements of the given matrix will not exceed 10,000.

### Solutions:

```java
Consider the indices of the diagonals of a NxM matrix. Let's use a 4x4 matrix as an example:

(0, 0) (0, 1) (0, 2) (0, 3)
(1, 0) (1, 1) (1, 2) (1, 3)
(2, 0) (2, 1) (2, 2) (2, 3)
(3, 0) (3, 1) (3, 2) (3, 3)
The first diagonal is (0, 0). The second is (0, 1), (1, 0), the third is (2, 0), (1, 1), (0, 2), etc.

It should be clear that the sum of row i and column j is equal to the index of the diagonal (diagonal number - 1). 
e.g. for the second diagonal (index 1), all possible pairings of (i, j) sum to 1, i.e. i + j = 1 for the 2nd 
diagonal. The maximum diagonal index is simply ((N-1) + (M-1)) = N + M - 2

So to solve the problem we simply need iterate through all possible diagonal indices (denote this as s) and find 
all possible pairs (i, j) such that i + j = s. The only thing we need to concern ourselves about is the order.
 We can find the ordering by looking at whether the diagonal index is even or odd. When the diagonal index is 
 even we want to the first pair to be (s, 0) and when it is odd when want the first pair to be (0, s), and 
 we decrease or increase i/j by 1 accordingly.

vector<int> findDiagonalOrder(vector<vector<int>>& matrix) {
        if(matrix.empty()) return {};

        const int N = matrix.size();
        const int M = matrix[0].size();

        vector<int> res;
        for(int s = 0; s <= N + M - 2; ++s)
        {
            // for all i + j = s
            for(int x = 0; x <= s; ++x) 
            {
                int i = x;
                int j = s - i;
                if(s % 2 == 0) swap(i, j);

                if(i >= N || j >= M) continue;

                res.push_back(matrix[i][j]);
            }
        }

        return res;
    }
```

\[0,0\] -&gt; \[0,1\], \[1,0\] -&gt; \[2,0\],\[1,1\],\[0,2\] -&gt; \[1,2\],\[2,1\] -&gt; \[2,2\]

```java
public class Solution {
    public int[] findDiagonalOrder(int[][] matrix) {
        if (matrix == null || matrix.length == 0) {
            int[] result = new int[0];
            return result;
        }
        int[] result = new int[matrix.length * matrix[0].length];
        int m = matrix.length, n = matrix[0].length, k = 0;
        for (int i = 0; i < m + n - 1; ++i) {
            int low = Math.max(0, i - n + 1), high = Math.min(i, m - 1);
            if (i % 2 == 0) {
                for (int j = high; j >= low; --j) {
                    result[k++] = matrix[j][i - j];
                }
            } else {
                for (int j = low; j <= high; ++j) {
                    result[k++] = matrix[j][i - j];
                }
            }
        }
        return result;
    }
}
```



