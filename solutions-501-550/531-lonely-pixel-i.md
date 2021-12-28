# 531 Lonely Pixel I

Given a picture consisting of black and white pixels, find the number of black lonely pixels.

The picture is represented by a 2D char array consisting of 'B' and 'W', which means black and white pixels respectively.

A black lonely pixel is character 'B' that located at a specific position where the same row and same column don't have any other black pixels.

Example:

```
Input: 
[['W', 'W', 'B'],
 ['W', 'B', 'W'],
 ['B', 'W', 'W']]

Output: 3
Explanation: All the three 'B's are black lonely pixels.
```

Note:  
1. The range of width and height of the input 2D array is \[1,500\].

### Solutions:

```java
/**
 * suppose matrix is m*n, there is at most min(m, n) lonely pixels, because there could be no more than 1 in each row, or column;
 * therefore, if we record num of black pixel on each row and column, we can easily tell whether each pixel is lonely or NO.
 *     _0_1_2_
 *  0 | 0 0 1   rows[0] = 1
 *  1 | 0 1 0   rows[1] = 1
 *  2 | 1 0 0   rows[2] = 1
 * 
 * cols[0][1][2]
 *     1  1  1
 */
class Solution {
public:
    int findLonelyPixel(vector<vector<char>>& pic) {
        int m = pic.size();
        int n = pic[0].size();
        vector<int> rows = vector<int>(m, 0);
        vector<int> cols = vector<int>(n, 0);
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                rows[i] += pic[i][j] == 'B';
                cols[j] += pic[i][j] == 'B';
            }
        }
        int lonely = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n && rows[i] > 0; j++) {
                lonely += pic[i][j] == 'B' && rows[i] == 1 && cols[j] == 1;
            }
        }
        return lonely;
    }
};
```



