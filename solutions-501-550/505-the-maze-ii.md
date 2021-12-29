# 505. The Maze II

### Problem:

There is a ball in a maze with empty spaces and walls. The ball can go through empty spaces by rolling up, down, left or right, but it won't stop rolling until hitting a wall. When the ball stops, it could choose the next direction.

Given the ball's start position, the destination and the maze, find the shortest distance for the ball to stop at the destination. The distance is defined by the number of empty spaces traveled by the ball from the start position \(excluded\) to the destination \(included\). If the ball cannot stop at the destination, return -1.

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

Output: 12
Explanation: One shortest way is : left -> down -> left -> down -> right -> down -> right.
             The total distance is 1 + 1 + 3 + 1 + 2 + 2 + 2 = 12.
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

Output: -1
Explanation: There is no way for the ball to stop at the destination.
```

![](/assets/maze_1_example_2.png)

Note:  
1. There is only one ball and one destination in the maze.  
2. Both the ball and the destination exist on an empty space, and they will not be at the same position initially.  
3. The given maze does not contain border \(like the red rectangle in the example pictures\), but you could assume the border of the maze are all walls.  
4. The maze contains at least 2 empty spaces, and both the width and height of the maze won't exceed 100.

### Solutions:

```java
/*We need to explore ALL the paths and find the shortest distance, but we can prune paths where the distance to reach a square is more than the currently shortest distance.
Because of that, we can use either BFS or DFS since it doesnt matter as we need to explore ALL the paths.. The main difference is when each path will be pruned. In the average case, BFS would be a much better result cause more paths will be pruned since the first few moves to a particular square will generally yield the shortest distance.

We use a distance[][] array that keeps track of the minimum distance to reach that square..
And prune any visits to that square if the distance is more than the minimum distance.*/

public class Solution {
    int[][] steps = new int[][]{{-1,0}, {1, 0}, {0, -1}, {0, 1}}; //up down left right


    public int shortestDistance(int[][] maze, int[] start, int[] destination) {
        int m = maze.length;
        int n = maze[0].length;

        int[][] distance = new int[m][n];  //can also use a hashmap
        for(int i = 0; i < m; i++) {
            Arrays.fill(distance[i], Integer.MAX_VALUE);
        }

        Queue<int[]> queue = new LinkedList();
        distance[start[0]][start[1]] = 0;
        queue.add(start);
        while (!queue.isEmpty()) {
            int[] pos = queue.poll();
            for (int i=0; i<4; i++) {
                int[] newPos = move(i, pos[0], pos[1], maze);
                int totalDistance = distance[pos[0]][pos[1]] + newPos[2];
                if (totalDistance < distance[newPos[0]][newPos[1]]) {
                    distance[newPos[0]][newPos[1]]  =  totalDistance;
                    if (newPos[0] == destination[0] && newPos[1] == destination[1]) { 
                        continue;
                    }
                    queue.add(newPos);
                }
            }
        }
        int shortest_distance = distance[destination[0]][destination[1]];
        return shortest_distance == Integer.MAX_VALUE ? -1 : shortest_distance; 
    }

    public int[] move(int dir,  int x, int y, int[][] maze) {
        int[] pos = new int[]{x, y, 0};
        while (isValid(maze, pos[0] + steps[dir][0] , pos[1] +  steps[dir][1])) {
            pos[0] += steps[dir][0];
            pos[1] += steps[dir][1];
            pos[2] += 1;
        }

        return pos;
    }


    public boolean isValid(int[][] maze, int x, int y) {
        if (!(x>=0 && y >=0 && x < maze.length && y < maze[0].length)) { return false; }
        return maze[x][y] != 1; //not a wall
    }

}
Another way to look at it is to model this as a Graph problem with weighted edges. Thus we want to find the shortest distance from a single source to the goal. Thus, it is the shortest path problem.
This can be solved by Dijkstra's Algorithm. This is similar to what we did previously. Except that

It uses a Priority Queue instead of a normal Queue to find the Node with the least distance from the starting point
Once that node is pop out from the queue, we know that the distance is definately the LEAST from the starting point and that value cannot be altered anymore. Thus, it can be marked as visited.
Thus, we can terminate once we the destination node is polled from the queue. If that doesnt happen, means it didnt reach the destination
 public int shortestDistance(int[][] maze, int[] start, int[] destination) {
        int m = maze.length;
        int n = maze[0].length;

        int[][] distance = new int[m][n];  //can also use a hashmap
        for (int i = 0; i < m; i++) {
            Arrays.fill(distance[i], Integer.MAX_VALUE);
        }

        PriorityQueue<int[]> pq = new PriorityQueue<>((p1, p2) -> p1[2] - p2[2]); 
        distance[start[0]][start[1]] = 0;
        pq.add(new int[]{start[0], start[1], 0});

        while (!pq.isEmpty()) {
            int[] pos = pq.poll();
            //visit.. Optional but help decrease runtime... 
            if (maze[pos[0]][pos[1]] == 2) { continue; }  //this is here because we might have inserted the same node twice in the PQ.
            maze[pos[0]][pos[1]] = 2; 

            if (pos[0] == destination[0] && pos[1] == destination[1]) { 
                return distance[pos[0]][pos[1]]; //this is now the shortest distance in the pq. 
                //thus, this IS the shortest distance from the source to the destination.
            }
            for (int i=0; i<4; i++) {
                int[] newPos = move(i, pos[0], pos[1], maze);
                int totalDistance = distance[pos[0]][pos[1]] + newPos[2];
                if (totalDistance < distance[newPos[0]][newPos[1]] && 
                    maze[newPos[0]][newPos[1]] != 2) {  //not visited.. dont need to add visited node into queue anymore, since we already foudn their shortest distance
                    distance[newPos[0]][newPos[1]]  =  totalDistance;
                    newPos[2] = totalDistance;
                    pq.add(newPos);
                }
            }
        }
        return -1;  // Does not reach destination
    }

Dijkstra's Algo seems to be an optimization of the first solution, since
1.we always select the node with the least cost
2. do not revisit visited nodes.. We might revisit nodes multiple times in the first solution..
3. terminate straight away when we find the destination.

However, the running time for the Dijkstra's algo is around 100-150ms while the first solution is 90-100ms.
Does anyone know why?
```



