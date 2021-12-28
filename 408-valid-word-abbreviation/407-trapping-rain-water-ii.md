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
```



