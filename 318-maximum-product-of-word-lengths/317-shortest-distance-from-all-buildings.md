# 317. Shortest Distance from All Buildings

You want to build a house on an empty land which reaches all buildings in the shortest amount of distance. You can only move up, down, left and right. You are given a 2D grid of values 0, 1 or 2, where:

Each 0 marks an empty land which you can pass by freely.  
Each 1 marks a building which you cannot pass through.  
Each 2 marks an obstacle which you cannot pass through.  
For example, given three buildings at \(0,0\), \(0,4\), \(2,2\), and an obstacle at \(0,2\):

```
1 - 0 - 2 - 0 - 1
|   |   |   |   |
0 - 0 - 0 - 0 - 0
|   |   |   |   |
0 - 0 - 1 - 0 - 0
```

The point \(1,2\) is an ideal empty land to build a house, as the total travel distance of 3+3+1=7 is minimal. So return 7.

### Solutions:

```java
//This keeps track of distances of each empty lands from all available buildings
    //For example, if grid[x][y] is 2, 4, 5 away from building 1, 2, 3 respectively,
    //its dis[x][y] = 2 + 4 + 5 = 11
    //once we do this for all available spaces, we can take min
    vector<vector<int>> distance(rows, vector<int>(cols, 0));
    
    //Complementary to dis[x][y]. We will only take min of dis[x][y] if the land
    //is accessible from *all* buildings. So in the above example, we will only consider
    // dis[x][y] iff reach[x][y] == 3
    vector<vector<int>> reach(rows, vector<int>(cols,0));
    
    //counts number of buildings present in the grid
    int buildings = 0;
    
    //traverse the whole grid now
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            if (grid[i][j] == 1) { //we found a building 
                buildings++;
                
                //cout << "Found Building @(" << i << "," << j << ")" << endl;
                
                //from this location, we do BFS for all empty locations
                //and update their distance from this building
                int distance_from_building = 1;
                vector<vector<bool>> visited(rows, vector<bool>(cols, false));
                queue<pair<int, int>> bfs;
                
                bfs.push(make_pair(i, j)); //push the building
                
                while (bfs.size()) {
                    int size = bfs.size();
                    
                    //cout << "@distance: " << distance_from_building << endl;
                    
                    for (int k = 0; k < size; k++) {
                        pair<int, int> coord = bfs.front();
                        bfs.pop();
                        
                        int x = coord.first;
                        int y = coord.second;
                        visited[x][y] = true;
                        
                        //check 4 corners from this node
                        //if it is a valid space:
                        // 1. Mark it visited, so it won't be considered again
                        // 2. Inremement 'reach' count
                        // 3. Update the distance
                        // 4. push the location for the next iteration
                        if (is_valid(grid, visited, x - 1, y)) {
                            //cout << x - 1 << "," << y << " valid" << endl;
                            visited[x - 1][y] = true;
                            reach[x - 1][y]++;
                            distance[x - 1][y] += distance_from_building;
                            bfs.push(make_pair(x - 1, y));
                        }
                        
                        if (is_valid(grid, visited, x + 1, y)) {
                            //cout << x + 1 << "," << y << " valid" << endl;
                            visited[x + 1][y] = true;
                            reach[x + 1][y]++;
                            distance[x + 1][y] += distance_from_building;
                            bfs.push(make_pair(x + 1, y));
                        }
                        
                        
                        if (is_valid(grid, visited, x, y - 1)) {
                            //cout << x << "," << y - 1 << " valid" << endl;
                            visited[x][y - 1] = true;
                            reach[x][y - 1]++;
                            distance[x][y - 1] += distance_from_building;
                            bfs.push(make_pair(x, y - 1));
                        }
                        
                        if (is_valid(grid, visited, x, y + 1)) {
                            //cout << x << "," << y + 1 << " valid" << endl;
                            visited[x][y + 1] = true;
                            reach[x][y + 1]++;
                            distance[x][y + 1] += distance_from_building;
                            bfs.push(make_pair(x, y + 1));
                        }
                    }
                    
                    distance_from_building++;
                }
            }
        }
    }
    
    //cout << "Done with BFS";
    
    int min_distance = INT_MAX;
    //now we scan the grid again and check for minimum value
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            //consider all locations that are
            // 1. empty lands
            // 2. Are accessible from all buildings
            if (grid[i][j] == 0 && reach[i][j] == buildings) {
                min_distance = min(min_distance, distance[i][j]);
            }
        }
    }
    
    if (min_distance == INT_MAX) return -1; // we did not find any empty land
    return min_distance;
    
}
```

```java
public class Solution {
    public int shortestDistance(int[][] grid) {
        if (grid == null || grid.length == 0 || grid[0].length == 0) {
            return -1;
        }
        int result = Integer.MAX_VALUE, look = 0;
        int[][] sums = new int[grid.length][grid[0].length];
        int[][] dirs = new int[][]{
           {0,-1},{-1,0},{0,1},{1,0}
           };
        for (int i = 0; i < grid.length; i ++) {
            for (int j = 0; j < grid[0].length; j ++) {
                if (grid[i][j] == 1) {
                    result = Integer.MAX_VALUE;
                    Queue<Integer> xs = new LinkedList<Integer>();
                    Queue<Integer> ys = new LinkedList<Integer>();
                    Queue<Integer> dis = new LinkedList<Integer>();
                    xs.add(i);
                    ys.add(j);
                    dis.add(0);
                    while (!xs.isEmpty()) {
                        int x = xs.poll();
                        int y = ys.poll();
                        int d = dis.poll();
                        for (int k = 0; k < dirs.length; k ++) {
                            int nX = x + dirs[k][0];
                            int nY = y + dirs[k][1];
                            if (nX >= 0 && nX < grid.length && nY >= 0 && nY < grid[0].length && grid[nX][nY] == look) {
                                grid[nX][nY] --;
                                sums[nX][nY] += d + 1;
                                xs.add(nX);
                                ys.add(nY);
                                dis.add(d + 1);
                                result = Math.min(result, sums[nX][nY]);
                            }
                        }
                    }
                    look --;
                }
            }
        }


        return result == Integer.MAX_VALUE ? -1 : result;
    }
}
```



