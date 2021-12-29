# 361 Bomb Enemy

### Problem:


Given a 2D grid, each cell is either a wall 'W', an enemy 'E' or empty '0' (the number zero), return the maximum enemies you can kill using one bomb.
The bomb kills all the enemies in the same row and column from the planted point until it hits the wall since the wall is too strong to be destroyed.
Note that you can only put the bomb at an empty cell.

Example:
For the given grid
<pre>
0 E 0 0
E 0 W E
0 E 0 0
</pre>


return 3. (Placing a bomb at (1,1) kills 3 enemies)


### Solutions:

```java

// Idea is to pre compute how many enemies are there in every direction before a wall comes then we just need to add these numbers
class Solution {
public:
    int maxKilledEnemies(vector<vector<char>>& grid) {
         int n = grid.size();
        int m = grid[0].size();
        int answer = 0;
        vector<vector<int>> row_left(n, vector<int>(m, 0));
        vector<vector<int>> row_right(n, vector<int>(m, 0));
        vector<vector<int>> col_top(n, vector<int>(m, 0));
        vector<vector<int>> col_down(n, vector<int>(m, 0));
        for (int i = 0; i < n; i++)
        {
            int val = 0;
            for (int j = 0; j < m; j++)
            {
                if (grid[i][j] == 'W')
                    val = 0;
                else if (grid[i][j] == 'E')
                    val++;
                else
                    row_left[i][j] = val;
            }
        }
        for (int i = 0; i < n; i++)
        {
            int val = 0;
            for (int j = m - 1; j >= 0; j--)
            {
                if (grid[i][j] == 'W')
                    val = 0;
                else if (grid[i][j] == 'E')
                    val++;
                else
                    row_right[i][j] = val;
            }
        }
        for (int j = 0; j < m; j++)
        {
            int val = 0;
            for (int i = 0; i < n; i++)
            {
                if (grid[i][j] == 'W')
                    val = 0;
                else if (grid[i][j] == 'E')
                    val++;
                else
                    col_top[i][j] = val;
            }
        }
        for (int j = 0; j < m; j++)
        {
            int val = 0;
            for (int i = n - 1; i >= 0; i--)
            {
                if (grid[i][j] == 'W')
                    val = 0;
                else if (grid[i][j] == 'E')
                    val++;
                else
                    col_down[i][j] = val;
            }
        }
        for (int i = 0; i < n; i++)
        {
            for (int j = 0; j < m; j++)
            {
                if (grid[i][j] != '0')
                    continue;
                answer = max(answer, row_left[i][j] + row_right[i][j] + col_down[i][j] + col_top[i][j]);
            }
        }
        return answer;
        
    }
};
```