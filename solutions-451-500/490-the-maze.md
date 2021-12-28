# 490 The Maze

### Problem:

There is a ball in a maze with empty spaces and walls. The ball can go through empty spaces by rolling up, down, left or right, but it won't stop rolling until hitting a wall. When the ball stops, it could choose the next direction.

Given the ball's start position, the destination and the maze, determine whether the ball could stop at the destination.

The maze is represented by a binary 2D array. 1 means the wall and 0 means the empty space. You may assume that the borders of the maze are all walls. The start and destination coordinates are represented by row and column indexes.

Example 1

```
Input 1: a maze represented by a 2D array

0 0 1 0 0
0 0 0 0 0
0 0 0 1 0
1 1 0 1 1
0 0 0 0 0

Input 2: start coordinate (rowStart, colStart) = (0, 4)
Input 3: destination coordinate (rowDest, colDest) = (4, 4)

Output: true
Explanation: One possible way is : left -> down -> left -> down -> right -> down -> right.
```

![](/assets/maze_1_example_1.png)

Example 2

```
Input 1: a maze represented by a 2D array

0 0 1 0 0
0 0 0 0 0
0 0 0 1 0
1 1 0 1 1
0 0 0 0 0

Input 2: start coordinate (rowStart, colStart) = (0, 4)
Input 3: destination coordinate (rowDest, colDest) = (3, 2)

Output: false
Explanation: There is no way for the ball to stop at the destination.
```

![](/assets/maze_1_example_2.png)

Note:  
1. There is only one ball and one destination in the maze.  
2. Both the ball and the destination exist on an empty space, and they will not be at the same position initially.  
3. The given maze does not contain border \(like the red rectangle in the example pictures\), but you could assume the border of the maze are all walls.  
4. The maze contains at least 2 empty spaces, and both the width and height of the maze won't exceed 100.

### Solutions:

```py
Explanation: Put the starting coordinates in your queue. For each element in your queue, you can move 4 different directions. The while loop makes your ball continue "rolling" right before it will hit a wall or roll off the boundaries of the maze. For each coordinate your ball has "stopped" on, put it into a set of coordinates that you've seen. If you don't do this, you'll end up with TLE because coordinates will continue to accumulate in your queue.

Runtime: O(RC), you might have to go through the entire board before you get to your destination.
Space: O(RC), worst case you have all "seen" elements in your set before you return.

It is only in the 50% for runtime, so not sure how I can make this faster, if you have any suggestions please let me know!

class Solution(object):
    def hasPath(self, maze, start, destination):
        """
        :type maze: List[List[int]]
        :type start: List[int]
        :type destination: List[int]
        :rtype: bool
        """
        R, C = len(maze), len(maze[0])
        q = collections.deque()
        q.append(start)
        seen = set(start)
        while q:
            for each in range(len(q)):
                x, y = q.popleft()
                for each in [1,0],[0,1],[-1,0],[0,-1]:
                    tempx, tempy, xo, yo = x, y, each[0], each[1]
                    while 0 <= tempx + xo < R and 0 <= tempy + yo < C and maze[tempx+ xo][tempy + yo] != 1: 
                        tempx, tempy = tempx + xo, tempy + yo
                    if (tempx, tempy) not in seen:
                        q.append([tempx, tempy])
                        seen.add((tempx, tempy))
                        if [tempx, tempy] == destination: return True
        return False 
                        
```



