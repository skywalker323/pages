# 533 Lonely Pixel II

### Problem:

Given a picture consisting of black and white pixels, and a positive integer N, find the number of black pixels located at some specific row R and column C that align with all the following rules:

Row R and column C both contain exactly N black pixels.  
For all rows that have a black pixel at column C, they should be exactly the same as row R  
The picture is represented by a 2D char array consisting of 'B' and 'W', which means black and white pixels respectively.

Example:

```
Input:                                            
[['W', 'B', 'W', 'B', 'B', 'W'],    
 ['W', 'B', 'W', 'B', 'B', 'W'],    
 ['W', 'B', 'W', 'B', 'B', 'W'],    
 ['W', 'W', 'B', 'W', 'B', 'W']] 

N = 3
Output: 6
Explanation: All the bold 'B' are the black pixels we need (all 'B's at column 1 and 3).
        0    1    2    3    4    5         column index                                            
0    [['W', 'B', 'W', 'B', 'B', 'W'],    
1     ['W', 'B', 'W', 'B', 'B', 'W'],    
2     ['W', 'B', 'W', 'B', 'B', 'W'],    
3     ['W', 'W', 'B', 'W', 'B', 'W']]    
row index

Take 'B' at row R = 0 and column C = 1 as an example:
Rule 1, row R = 0 and column C = 1 both have exactly N = 3 black pixels. 
Rule 2, the rows have black pixel at column C = 1 are row 0, row 1 and row 2. They are exactly the same as row R = 0.
```

Note:  
1. The range of width and height of the input 2D array is \[1,200\].

### Solutions:

```java

/**
 * Steps:
 * >> 1. create map<int, set<int>> cols, rows; -- to store black dots on that row;
 * 
 *     _0_1_2_3_4_5_
 *  0 | 0 l 0 1 1 0     rows[0] = {1, 3, 4}
 *  1 | 0 l 0 1 1 0     rows[1] = {1, 3, 4}
 *  2 | 0 l 0 1 1 0     rows[2] = {1, 3, 4}
 *  3 | 0 0 1 0 1 0     rows[3] = {  2,  4}
 * 
 * >> 2. for every pixel meet rule 1, that is: pic[i][j] == 'B' && rows[i].size() == N && cols[j].size() == N
 *       check rule2: for every row k in cols[j];  check that row[k] = row[i];
 * 
 * We can tell the 6 black pixel in col 1 and col 3 are lonely pixels
 *     _0_1_2_3_4_5_
 *  0 | 0 L 0 L 1 0     rows[0] = {1, 3, 4}  =
 *  1 | 0 L 0 L 1 0     rows[1] = {1, 3, 4}  =
 *  2 | 0 L 0 L 1 0     rows[2] = {1, 3, 4} 
 *  3 | 0 0 1 0 1 0     rows[3] = {  2,  4}
 *
 */
 
 class Solution {
public:
    int findBlackPixel(vector<vector<char>>& pic, int N) {
        int m = pic.size();
        int n = pic[0].size();
        unordered_map<int, set<int>> rows;  // black pixels in each row
        unordered_map<int, set<int>> cols;  // black pixels in each col
        /* 1. create map<int, set<int>> cols, rows; -- to store black dots on that row; */
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (pic[i][j] == 'B') {
                    rows[i].insert(j);
                    cols[j].insert(i);
                }
            }
        }
        /* 2. for every pixel meet rule 1: pic[i][j] == 'B' && rows[i].size() == N && cols[j].size() == N */
        int lonelys = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n && rows.count(i); j++) {
                if (pic[i][j] == 'B' && rows[i].size() == N && cols[j].size() == N) {   // rule 1 fulfilled
                    /* check rule2: for every row k in cols[j];  check that row[k] = row[i]; */
                    bool lonely = true;
                    for (int r : cols[j]) {
                        if (rows[r] != rows[i]) {
                            lonely = false; break;
                        }
                    }
                    lonelys += lonely;
                }
            }
        }
        return lonelys;
    }
};


```

```java
class Solution {
    public int findBlackPixel(char[][] picture, int N) {
        char[][] pic = picture;
        if (pic == null || pic.length == 0 || pic[0].length == 0) {
            return 0;
        }
        int[] row = new int[pic.length];
        int[] col = new int[pic[0].length];
        for (int i = 0; i < pic.length; i ++) {
            for (int j = 0; j < pic[0].length; j ++) {
                if (pic[i][j] == 'B') {
                    row[i] ++;
                    col[j] ++;
                }
            }
        }
        boolean[] invalidCol = new boolean[pic[0].length];
        for (int j = 0; j < invalidCol.length; j ++) {
            if (col[j] != N) {
                invalidCol[j] = true;
            }
        }
        for (int j = 0; j < pic[0].length; j ++) {
            String samerow = null;
            System.out.println("checking col " + j);
            for (int i = 0; i < pic.length; i ++) {
                if (pic[i][j] == 'B') {
                    if (row[i] != N) {
                        invalidCol[j] = true;
                        break;
                    }
                    if (samerow == null) {
                        samerow = new String(pic[i]);
                    }
                    else {
                        if (!samerow.equals(new String(pic[i]))) {
                            invalidCol[j] = true;
                            break;
                        }
                    }
                }
            }
        }
        int count = 0;
        for (int j = 0; j < invalidCol.length; j ++) {
            if (invalidCol[j] == false) {
                count += N;
            }
        }
        return count;
    }
}
```



