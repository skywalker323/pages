# 130 Surrounded Regions – Medium

### Problem:

Given a 2D board containing 'X' and 'O', capture all regions surrounded by 'X'.

A region is captured by flipping all 'O's into 'X's in that surrounded region.

For example,

X X X X  
X O O X  
X X O X  
X O X X  
After running your function, the board should be:

X X X X  
X X X X  
X X X X  
X O X X

Explanation: Surrounded regions should not be on the border, which means that any 'O' on the border of the board are not flipped to 'X'. Any 'O' that is not on the border and it is not connected to an 'O' on the border will be flipped to 'X'. Two cells are connected if they are adjacent cells connected horizontally or vertically.

### Thoughts:

This problem is more like a DFS problem. Solution is using the idea of a DFS, but in a “BFS” way.

Here is the steps of the solution.

1 Find loc for all the ‘O’ at the edge of the square, push these loc into a queue.

2 Iterate over the queue, for each loc, go all possible loc it can reach and marked all the loc with “Y” so that unsurrounded ‘O’ becomes “Y” while surrounded ‘O’ stays ‘O’.

3 Flip all the ‘O’ to be ‘X’ and ‘Y’ to be ‘O’ and we are done.

So the whole ideal is to like a DFS, go deeper as possible. But not like a normal DFS that first iteration is on every elements, this one is on all edged ‘O’ in a queue. Only because it’s using a queue so I called it a “BFS” way.

### Solutions:

Alternative solution:

```cpp
class Solution {
public:
    vector<int>dx{-1,1,0,0};
    vector<int>dy{0,0,1,-1};
    void dfs(int row, int col, vector<vector<char>>& board){
        board[row][col] = 'N';
        
        for(int i=0;i<4;i++){
            int ni = row + dx[i];
            int nj = col + dy[i];
            
            if(ni >= 0 and ni < board.size() and nj >= 0 and nj < board[0].size() and board[ni][nj] == 'O'){
                dfs(ni, nj, board);
            }
        }
    }
    
    void solve(vector<vector<char>>& board) {
        int n = board.size();
        for(int i=0;i<board[0].size();i++){
            if(board[0][i] == 'O') dfs(0, i, board);
            if(board[board.size() - 1][i] == 'O')dfs(board.size() - 1, i, board);
        }
        for(int i=0;i<board.size();i++){
            if(board[i][0] == 'O') dfs(i, 0, board);
            if(board[i][board[0].size() - 1] == 'O')dfs(i, board[0].size() - 1, board);
        }
        
        for(int i=0;i<board.size();i++){
            for(int j=0;j<board[0].size();j++){
                if(board[i][j] == 'N') board[i][j] = 'O';
                else if(board[i][j] == 'O') board[i][j] = 'X';
            }
        }        
    }
};
```



