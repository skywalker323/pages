# 329 Longest Increasing Path in a Matrix

### Problem:

Given an integer matrix, find the length of the longest increasing path.

From each cell, you can either move to four directions: left, right, up or down. You may NOT move diagonally or move outside of the boundary \(i.e. wrap-around is not allowed\).

Example 1:

```
nums = [
  [9,9,4],
  [6,6,8],
  [2,1,1]
]
```

Return 4  
The longest increasing path is \[1, 2, 6, 9\].  
Example 2:

```
nums = [
  [3,4,5],
  [3,2,6],
  [2,2,1]
]
```

Return 4  
The longest increasing path is \[3, 4, 5, 6\]. Moving diagonally is not allowed.

# Solutions:

```java
int m,n;
public:
int getAllPossiblePathLen(vector<vector<int>>& mat,vector<vector<int>>& dp,int x,int y,int p){
    
    if(x < 0 || y < 0 || x >= m || y >= n || mat[x][y] == -1 || p >= mat[x][y]) return 0; // base cases
    
    if(dp[x][y] != -1) return dp[x][y]; // check if already existing 
    
    int num = mat[x][y];
    mat[x][y] = -1;  // marked visited
    
    int up    = getAllPossiblePathLen(mat,dp,x+1,y,num);
    int down  = getAllPossiblePathLen(mat,dp,x-1,y,num);
    int left  = getAllPossiblePathLen(mat,dp,x,y-1,num);
    int right = getAllPossiblePathLen(mat,dp,x,y+1,num);
    
    mat[x][y] = num; // backtracking 
    
    dp[x][y] = max(up, max(down, max(left, right))) + 1; // memomization
    
    return dp[x][y];
}

int longestIncreasingPath(vector<vector<int>>& matrix) {
    
    m = matrix.size();
    n = matrix[0].size();
    int Max_len = 0;
    vector<vector<int>> dp(m, vector<int>(n, -1));
    
    for(int i = 0;i< m;i++){
        for(int j = 0;j< n;j++){ 
            Max_len = max(Max_len, getAllPossiblePathLen(matrix,dp,i,j,-1));
        }
    }
    
    return Max_len;
}
```



