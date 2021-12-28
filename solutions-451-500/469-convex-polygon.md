# 469. Convex Polygon

### Problem:

Given a list of points that form a polygon when joined sequentially, find if this polygon is convex \(Convex polygon definition\).

Note:

There are at least 3 and at most 10,000 points.  
Coordinates are in the range -10,000 to 10,000.  
You may assume the polygon formed by given points is always a simple polygon \(Simple polygon definition\). In other words, we ensure that exactly two edges intersect at each vertex, and that edges otherwise don't intersect each other.

Example 1:

```
[[0,0],[0,1],[1,1],[1,0]]

Answer: True

Explanation:
```

![](/assets/1.png)

Example 2:

```
[[0,0],[0,10],[10,10],[10,0],[5,5]]

Answer: False

Explanation:
```

![](/assets/2.png)

### Solutions:

```java
Great solution inspired by @Ipeq1! Here is a C++ version with extracted determinant calculation.

The key observation for convexity is that vector pi+1-pi always turns to the same direction to pi+2-pi formed by any 3 sequentially adjacent vertices, i.e., cross product (pi+1-pi) x (pi+2-pi) does not change sign when traversing sequentially along polygon vertices.

Note that for any 2D vectors v1, v2,

v1 x v2 = det([v1, v2])
which is the determinant of 2x2 matrix [v1, v2]. And the sign of det([v1, v2]) represents the positive z-direction of right-hand system from v1 to v2. So det([v1, v2]) ≥ 0 if and only if v1 turns at most 180 degrees counterclockwise to v2.

Version 1: use pos, neg as 0-1 flags to store if positive or negative determinant has ever been encountered:

    bool isConvex(vector<vector<int>>& p) {
      for (int i=0, pos=0, neg=0, n=p.size(); i < n; ++i) {
        long det = det2({p[i], p[(i+1)%n], p[(i+2)%n]});
        if ((pos|=(det>0))*(neg|=(det<0))) return false;
      }    
      return true;
    }
    // determinant of 2x2 matrix [A1-A0, A2-A0]
    long det2(const vector<vector<int>>& A) {
    	return (A[1][0]-A[0][0])*(A[2][1]-A[0][1]) - (A[1][1]-A[0][1])*(A[2][0]-A[0][0]);
    }
Version 2: use prev and cur to store previous and current non-zero determinants:

    bool isConvex(vector<vector<int>>& p) {
      for (long i = 0, n = p.size(), prev = 0, cur; i < n; ++i)
        if (cur = det2({p[i], p[(i+1)%n], p[(i+2)%n]})) 
          if (cur*prev < 0) return false; 
          else prev = cur;

      return true;
    }
```



