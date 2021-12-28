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



int trap(vector<int>& height)
{
    int ans = 0, current = 0;
    stack<int> st;
    while (current < height.size()) {
        while (!st.empty() && height[current] > height[st.top()]) {
            int top = st.top();
            st.pop();
            if (st.empty())
                break;
            int distance = current - st.top() - 1;
            int bounded_height = min(height[current], height[st.top()]) - height[top];
            ans += distance * bounded_height;
        }
        st.push(current++);
    }
    return ans;
}
```



