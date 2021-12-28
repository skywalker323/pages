# 407 Trapping Rain Water II

### Problem:

Given an m x n matrix of positive integers representing the height of each unit cell in a 2D elevation map, compute the volume of water it is able to trap after raining.

Note:  
Both m and n are less than 110. The height of each unit cell is greater than 0 and is less than 20,000.

Example:

```
Given the following 3x6 height map:
[
  [1,4,3,1,3,2],
  [3,2,1,3,2,4],
  [2,3,3,2,3,1]
]

Return 4.
```

![](/assets/rainwater_empty.png)

The above image represents the elevation map \[\[1,4,3,1,3,2\],\[3,2,1,3,2,4\],\[2,3,3,2,3,1\]\] before the rain.

![](/assets/rainwater_fill.png)

After the rain, water are trapped between the blocks. The total volume of water trapped is 4.

### Solutions:

See [this article](http://www.cnblogs.com/grandyang/p/5928987.html)

```cpp
/* if the size of the heights vector is <=2 then no water will be stored ,no matter whatever is the size of the 
bar is.If we consider an infinite bar on the right and start storing water then the amount of water that will be
stored at each place will depend on the longest bar on the left side i.e (longest_bar_size-current_bar_size).
We will traverse from left to right assuming infinite bar on right side and store the amount of water trapped 
in a vector.
Again we will start traversing from the right to left .This time we can store (right_largest_bar-curr_bar_height) if
we consider an infinite bar on left,but we have already stored the amount we can store in the vector.

Now there is neither an infinite bar on left nor on right.So the maximum water that can be stored will be the 
minimum of the two amount.
/*//

int trap(vector<int>& height) {
        int n=height.size(),mx,ans=0;  //mx--->max_bar_height
        if(n<=2) return 0;
        mx=height[0];
        vector<int> vec(n,0);      
         //vector to store amount of water that can be stored initialised to zero
        for(int i=1;i<n;i++){       
        //traversing fron left to right
            if(height[i]>mx) mx=height[i]; 
            //if a bar is greater than the previous one then on that bar 
            // no amount of water can be stored,updating the max_bar
            else vec[i]=mx-height[i];    
            //amount that can be stored
        }
        mx=height[n-1];                   
        //rightmost bar as greatest
        for(int i=n-2;i>=0;i--){
            if(height[i]>mx) mx=height[i];    
             //updating the greatest bar
            else ans+=min(mx-height[i],vec[i]);  
            //storing the actual ans
        }
        return ans;
    }
    
    
Coming from Trapping Water I
Alright. So most people who try out Trapping Rain Water II after solving Trapping Rain Water I 
(including myself) reason out that the problem is similar except that now you need to check 4 directions 
compared to 2 directions (left and right) for each point. This infact is incorrect as in 2D, water
 can flow out of diagonally as well. Therefore, the amount of water that can be stored above a
  grid element depends on its neighboring rows and columns as well. A simple example to see which 
  this might be the case, check this example out

1 1 1 1 1 1 1
1 1 0 0 0 1 1
1 0 1 1 1 0 1
1 1 0 0 0 1 1
1 1 1 1 1 1 1
So for any grid item of height 0, you can see that water flows into the 0 cells along its 8
 adjacent directions. And this gets even more complicated to keep track with varying heights
  instead of having just 0 and 1.

So how do you solve it
The key observation to solving this problem comes from understanding that to be able to hold any
 sort of water over a grid element, you need one of the elements inside the grid to lie at a lower
  point and the lowest boundary of this low point dictates how much water it can hold. So we go 
  ahead and put the boundary of the grid in a priority queue.

Now what we do is, we pick the smallest element (call it A) out of the boundary. If its neighbor
 (call it B) is at a higher point than A, then B becomes a new part of the boundary. 
 Think of it like this. Imagine you had a lake surrounded by two walls. The outer wall is
  the smaller wall A and the inner wall B is the higher wall, the amount of water stored in 
  the lake is dictated by the higher inner wall B. So we add our grid element B to our priority queue.

Now of the other case. When A is greater than B. Lets go through this part slowly. We claimed A to
 be the smallest part of our boundary. Every other boundary is larger than A. And if you recall 
 the observation we made earlier "The key observation to solving this problem comes from understanding 
 that to be able to hold any sort of water over a grid element, you need one of the elements inside the grid
  to lie at a lower point and the lowest boundary of this low point dictates how much water it can hold." 
  There we go! Since A is the lowest boundary that point B can have, B will have B - A amount of water above
   it. And to any other point around B, the height of A will still be the boundary for it (since B is covered by A,
    if there is a point around B, lower than B, (call this point C); A will still be the boundary for C) 
    (If this point wasn't clear, think of when you are trying to store water in between a stairs like structure.

3 2 1 0 1 2 3 
If these are the heights of the stairs, the amount of water stored by 1 is not dictated by 2.
 It's dictated by 3 since 3 is the boundary for 1 and not 2


class Solution {
public:
    int trapRainWater(vector<vector<int>>& heightMap) {
        if(heightMap.size()==0) return 0;
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> que;
        int row = heightMap.size(), col = heightMap[0].size();
        vector<vector<int>> visited(row, vector<int>(col, 0));
        int ans = 0, Max = INT_MIN;
        for(int i = 0; i < row; i++)
        {
            for(int j = 0; j < col; j++)
            {
                if(!(i==0 || i==row-1 || j==0 || j==col-1)) continue;
                que.push(make_pair(heightMap[i][j], i*col+j));
                visited[i][j] = 1;
            }
        }
        vector<vector<int>> dir{{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
        while(!que.empty())
        {
            auto val = que.top(); que.pop();
            int height = val.first, x = val.second/col, y = val.second%col;
            Max = max(Max, height);
            for(auto d: dir)
            {
                int x2 = x + d[0], y2 = y + d[1];
                if(x2>=row || x2<0 || y2<0 || y2>=col || visited[x2][y2]) continue;
                visited[x2][y2] = 1;
                if(heightMap[x2][y2] < Max) ans += Max - heightMap[x2][y2];
                que.push(make_pair(heightMap[x2][y2], x2*col+y2));
            }
        }
        return ans;
    }
};
```



