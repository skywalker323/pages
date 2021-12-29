# 554 Brick Wall

### Problem

There is a brick wall in front of you. The wall is rectangular and has several rows of bricks. The bricks have the same height but different width. You want to draw a vertical line from the top to the bottom and cross the least bricks.

The brick wall is represented by a list of rows. Each row is a list of integers representing the width of each brick in this row from left to right.

If your line go through the edge of a brick, then the brick is not considered as crossed. You need to find out how to draw the line to cross the least bricks and return the number of crossed bricks.

You cannot draw a line just along one of the two vertical edges of the wall, in which case the line will obviously cross no bricks.

Example:

```
Input: 
[[1,2,2,1],
 [3,1,2],
 [1,3,2],
 [2,4],
 [3,1,2],
 [1,3,1,1]]
Output: 2
Explanation:
```

![](/assets/brick_wall.png)  
Note:  
The width sum of bricks in different rows are the same and won't exceed INT\_MAX.  
The number of bricks in each row is in range \[1,10,000\]. The height of wall is in range \[1,10,000\]. Total number of bricks of the wall won't exceed 20,000.

### Solutions

```java
Idea:
If the goal here is to find where a line will cross the fewest bricks, then we could also say that the goal is to
 find where the most brick edges line up. We can consider the edges to occur at an index representing the current |
 running total of the previous elements of a given row of the wall. For example, if the row is defined as [1,2,2,1],
  then the inside edges occur at [1,1+2,1+2+2], or [1,3,5].

If we now know how to find the edges, then we're left with finding out which index has the highest frequency of edges,
 which naturally calls for a frequency map.

So we can iterate through each row in the wall, keep a running total of the current row (rowSum), and then update 
the frequency of each edge's index in our frequency map (freq).

Once we're done filling freq, we just need to iterate through it to find the highest value (best), which represents 
the number of edges aligned on a single index. Our actual answer, however, is the number of bricks, not edges, 
so we should return the total number of rows minus best.

Implementation:
For Javascript, it's more performant to iterate through the finished freq looking for the best result

In Python it's easier to run max() directly on freq.

For Java and C++ it's faster to keep track of best as we add values to freq.

For Java, it's also weirdly more performant to extract the row processing to a helper function.


class Solution {
public:
    int leastBricks(vector<vector<int>>& wall) {
        unordered_map<int, int> freq;
        int best = 0;
        for (int i = 0; i < wall.size(); i++) {
            int rowSum = wall[i][0];
            for (int j = 1; j < wall[i].size(); j++) {
                freq[rowSum]++;
                best = max(best, freq[rowSum]);
                rowSum += wall[i][j];
            };
        };
        return wall.size() - best;
    };
};
```



