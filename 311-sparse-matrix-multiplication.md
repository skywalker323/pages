# 311 Sparse Matrix Multiplication

### Problem:

<pre>
Given two sparse matrices A and B, return the result of AB.

You may assume that A's column number is equal to B's row number.

Example:

A = [
  [ 1, 0, 0],
  [-1, 0, 3]
]

B = [
  [ 7, 0, 0 ],
  [ 0, 0, 0 ],
  [ 0, 0, 1 ]
]


     |  1 0 0 |   | 7 0 0 |   |  7 0 0 |
AB = | -1 0 3 | x | 0 0 0 | = | -7 0 3 |
                  | 0 0 1 |
            
</pre>

### Solutions:

```java
// Normal matrix multiplication

vector<vector<int>> multiply(vector<vector<int>>& mat1, vector<vector<int>>& mat2) {
        int m = mat1.size(), n = mat1[0].size(), q = mat2[0].size();
        vector<vector<int>> ans(m, vector<int>(q, 0));
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < q; j++) {
                for (int k = 0; k < n; k++) {
                    ans[i][j] += mat1[i][k] * mat2[k][j];
                }
            }
        }
        return ans;
    }
// Sparse matrix multiplication

class Solution {
public:
    vector<vector<int>> multiply(vector<vector<int>>& mat1, vector<vector<int>>& mat2) {
        int m = mat1.size(), n = mat1[0].size(), q = mat2[0].size();
        vector<vector<int>> ans(m, vector<int>(q, 0));
        unordered_map<int, unordered_map<int, int>> m1;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (mat1[i][j] != 0) {
                    m1[i][j] = mat1[i][j];
                }
            }
        }
        for (auto [r, rows] : m1) {
            for (int j = 0; j < q; j++) {
                for (auto [c, v] : rows) {
                    ans[r][j] += v * mat2[c][j];
                }
            }
        }
        return ans;
    }
};
/*

Although the matrix multiplication in traditional methods shall always have a time complexity of (n^3) , we need to find way to avoid the multiplication of zeroes. To do so, so we need to understand what are the different ways of matrix multiplication.

Other the tradtion matrix multiplication where we do ROW * COLUMN calculation, there is another way of doing so.

Here is a great link to understand the matrix multiplcation as linear combinations of columns https://dzone.com/articles/visualizing-matrix

Basically each column of the ouput matrix is a linear combination of all the columns in the first matrix with each of the columns in the second matrix.

image

As you can see in the above example how we did a linear combination of the first matrix with the second which effectively is a vector. Now if 'a' is 0 we can effectively skip the entirity of column 1 in matrix for the calculation of column1 in the ouput vector.

So, just consider a second matrix a set of vector and repeat the above process. */

class Solution {
public:

    vector<vector<int>> multiply(vector<vector<int>>& mat1, vector<vector<int>>& mat2) {

		// First create the output matrix
        vector<vector<int>> arr(mat1.size(), vector<int>(mat2[0].size(), 0));
        
		// Start iterating over each column of the SECOND MATRIX
        for(int c2 = 0 ; c2<mat2[0].size(); c2++){
		
		   // Iterate along each row of the SECOND MATRIX
            for(int r2 = 0 ; r2<mat2.size(); r2++){
			
			    // SKIP if the value is zero because the effective vector shall be 0
                if(mat2[r2][c2]!=0){
				
				   // Iterate over the rows of the first MATRIX
                    for(int r1=0;r1<mat1.size();r1++){
                        
                        arr[r1][c2] = arr[r1][c2] + mat2[r2][c2]* mat1[r1][r2]; 
                        
                    }   
                }
            }
        }
        return arr;
    }
};
```