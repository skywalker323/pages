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

```java
public class Solution {
    public void solve(char[][] board) {
        for (int i = 0; i < board.length; i ++) {
            mark(board, i, 0);
            mark(board, i, board[i].length - 1);
        }
        for (int j = 0; j < board[0].length; j ++) {
            mark(board, 0, j);
            mark(board, board.length - 1, j);
        }
        for (int i = 0; i < board.length; i ++) {
            for (int j = 0; j < board[0].length; j ++) {
                if (board[i][j] == 'O') {
                    board[i][j] = 'X';
                    continue;
                }
                if (board[i][j] == 'Y') {
                    board[i][j] = 'O';
                    continue;
                }
            }
        }
    }
    private void mark(char[][] board, int i, int j) {
        Queue<Integer> xs = new LinkedList<Integer>();
        Queue<Integer> ys = new LinkedList<Integer>();
        xs.add(i);
        ys.add(j);
        if (board[i][j] != 'O') {
            return;
        }
        while(xs.size() > 0) {
            int x = xs.poll();
            int y = ys.poll();
            board[x][y] = 'Y';
            if (x - 1 >= 0 && board[x-1][y] == 'O') {
                xs.add(x - 1);
                ys.add(y);
            }
            if (x + 1 < board.length && board[x+1][y] == 'O') {
                xs.add(x + 1);
                ys.add(y);
            }
            if (y - 1 >= 0 && board[x][y-1] == 'O') {
                xs.add(x);
                ys.add(y - 1);
            }
            if (y + 1 < board[0].length && board[x][y+1] == 'O') {
                xs.add(x);
                ys.add(y + 1);
            }
        }

    }
}
```



