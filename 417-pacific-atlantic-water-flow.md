# 417 Pacific Atlantic Water Flow

### Problem:

Given an m x n matrix of non-negative integers representing the height of each unit cell in a continent, the "Pacific ocean" touches the left and top edges of the matrix and the "Atlantic ocean" touches the right and bottom edges.

Water can only flow in four directions \(up, down, left, or right\) from a cell to another one with height equal or lower.

Find the list of grid coordinates where water can flow to both the Pacific and Atlantic ocean.

Note:  
The order of returned grid coordinates does not matter.  
Both m and n are less than 150.  
Example:

```
Given the following 5x5 matrix:

  Pacific ~   ~   ~   ~   ~ 
       ~  1   2   2   3  (5) *
       ~  3   2   3  (4) (4) *
       ~  2   4  (5)  3   1  *
       ~ (6) (7)  1   4   5  *
       ~ (5)  1   1   2   4  *
          *   *   *   *   * Atlantic

Return:

[[0, 4], [1, 3], [1, 4], [2, 2], [3, 0], [3, 1], [4, 0]] (positions with parentheses in above matrix).
```

### Solutions:

```cpp
 /*
     * Approach followed is to find the containers from where the water flows to
     * pacific ocean and containers from where the water flows to atlantic ocean.
     * Result is the indices of the container from where the water is flown to
     * either pacific or atlantic 
    */

    // DFS to find whether water from a specific container goes to the ocean i.e.., pacific or atlantic
    void dfs(vector<vector<int>>& matrix, int row, int col, int prevContainerVal, vector<vector<bool>>& ocean)
    {
        // Water can move from a container to another in either top, bottom, left or right
        // checking the cornor cases
        if (row < 0 || row >= matrix.size() || col < 0 || col >= matrix[0].size()) {
            // In ocean already
            return;
        }

        // water form a container to another can flow only when height is equal or lower
        if (prevContainerVal > matrix[row][col] || ocean[row][col]) {
            return;
        }
        //water can flow in the container, as it stasfies the required condition
        ocean[row][col] = true;

        // water can flow in 4 directions, 4 subproblems
        // top
        dfs(matrix, row - 1, col, matrix[row][col], ocean);
        // bottom
        dfs(matrix, row + 1, col, matrix[row][col], ocean);
        // left
        dfs(matrix, row, col - 1, matrix[row][col], ocean);
        // right
        dfs(matrix, row, col + 1, matrix[row][col], ocean);

        return;
    }

    vector<vector<int>> pacificAtlantic(vector<vector<int>>& matrix) {
        vector<vector<int>> res;

        if (matrix.empty() || matrix[0].size() == 0) {
            // There containers are empty there is no way to flow the water
            return res;
        }

        int row = matrix.size();
        int col = matrix[0].size();
        // Boolean matrix to store whether the water flows into ocean(pacific, atlantic)
        // from a specific container
        vector<vector<bool>> pacific(row, vector<bool>(col, false));
        vector<vector<bool>> atlantic(row, vector<bool>(col, false));

        // Check whether water int the container can flow to the ocean
        // first row(pacific) and last row(atlantic)
        for (int i = 0; i < col; i++) {
            dfs(matrix, 0, i, INT_MIN, pacific);
            dfs(matrix, row - 1, i, INT_MIN, atlantic);
        }

        // first column(pacific) and last column(atlantic)
        for (int i = 0; i < row; i++) {
            dfs(matrix, i, 0, INT_MIN, pacific);
            dfs(matrix, i, col - 1, INT_MIN, atlantic);
        }

        // Finding the coordinates where water flows from pacific to atlantic.
        for (int i = 0; i < row; i++) {
            for (int j = 0; j < col; j++) {
                // If the water in the i,j container flows to pacific and to atlantic, then
                // there is a result
                if (pacific[i][j] && atlantic[i][j]) {
                    res.push_back({ i,j });
                }
            }
        }

        // returning the coording where the water flows from pacific to atlantic
        return res;
    }
```

```java

```



