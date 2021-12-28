# 562 Longest Line of Consecutive One in Matrix

### Problem:

Given a 01 matrix M, find the longest line of consecutive one in the matrix. The line could be horizontal, vertical, diagonal or anti-diagonal.

Example:

```
Input:
[[0,1,1,0],
 [0,1,1,0],
 [0,0,0,1]]
Output: 3
```

Hint: The number of elements in the given matrix will not exceed 10,000.

### Solutions:

```java
3
rock1270's avatar
rock1270
6
Last Edit: October 11, 2018 11:59 PM

1.6K VIEWS

intuition:
From each non-zero point think about going forward in all 4 directions (right, down, diagonal towards right end, 
anti-diagonal towards left end), while tranversing in any direction count the number of 1s.

To make it O(mn), instead of every non-zero point, only tranverse from a "starting point". A starting point is 
either boundary or a zero point in opposite direction (look at the comments in the function IsStartingPoint). 
This way we are not tranversing any item twice.

public class Solution {
    
     //  go right, godown, go diagonal towards right end, go diagonal towards left end (anti diagonal) 
    int[][] dirs = new int [][] { new int[] {0,1} , new int[] {1,0}, new int[] {1,1}, new int[] {1,-1} };
        
    // constant space
    // if we are at a starting point then tranverse all direction and count
    // checking starting point ensures that we are not repeating and hence O(mn).
    public int LongestLine(int[,] M){
        
        var answer = 0;        
        for(int r = 0; r < M.GetLength(0); r++){
            for(int c = 0; c < M.GetLength(1); c++){
                if(M[r,c] == 1){
                    for(int d = 0; d < 4; d++){
                        if(IsStartingPoint(r,c,M, d)){
                            // count all 1 in this direction
                            var tempR =r;
                            var tempC = c;
                            var oneInThisDirection = 0;
			   // Dont worry about this while loop, items include in this loop will be excluded next time 
			   // since their IsStartingPoint will return false;
                            while(tempR < M.GetLength(0) && tempC < M.GetLength(1) && tempC >= 0 
                                           && M[tempR,tempC] == 1){
                                oneInThisDirection++;
                                tempR += dirs[d][0];
                                tempC += dirs[d][1];
                            }
                            
                            answer = Math.Max(answer, oneInThisDirection);
                        }
                    }
                }
            }            
        }
        
        return answer;
    }
    
    private bool IsStartingPoint(int r, int c, int[,] M, int dir){
	// newR and newC are points in opposite direction
	// e.g. at M[2,2] and direction = down, look for Up Direction i.e. M[2,1]
	// if M[2,1] is 0 then its a starting point else false
        int newr = r-dirs[dir][0];
        int newc = c-dirs[dir][1];
        
        // if boundary then it is a starting point
        if(newr < 0 || newc < 0 || newc >= M.GetLength(1)){
            return true;
        }
        
        // else is this is the first one in this direction?
        return M[newr, newc] == 0;
    }
}
```



