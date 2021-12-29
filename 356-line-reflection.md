# 356 Line Reflection

### Problem:

Given n points on a 2D plane, find if there is such a line parallel to y-axis that reflect the given points.

Example 1:  
Given points = \[\[1,1\],\[-1,1\]\], return true.

Example 2:  
Given points = \[\[1,1\],\[-1,-1\]\], return false.

Follow up:  
Could you do better than O\(n2\)?

Hint:

Find the smallest and largest x-value for all points.  
If there is a line then it should be at y = \(minX + maxX\) / 2.  
For each point, make sure that it has a reflected point in the opposite side.

### Solutions:

```java
class Solution {
public:
    bool isReflected(vector<vector<int>>& points) {
        int minx = points[0][0], maxx = points[0][0];
        
        for (auto& pt : points){
            minx = min(minx, pt[0]);
            maxx = max(maxx, pt[0]);
        }
        
        int chosen = maxx+minx; //avoid float by not dividing 2
        
        unordered_map<int, unordered_map<int, int>> m; //[y][abs(2*x-chosen)]:cnt        
        
        unordered_set<int> leftshowed, rightshowed; //used for checking replicas
        
        for (auto& pt : points){ 
            if (pt[0]*2 == chosen) continue;  //on the chosen line
            
            if (pt[0]*2 > chosen) { //right side
                if (rightshowed.count(pt[0])) continue; //ignore replicas
                
                m[pt[1]][(pt[0]*2)-chosen]++;
                rightshowed.insert(pt[0]);
            }else{ //left side
                if (leftshowed.count(pt[0])) continue;
                
                m[pt[1]][chosen-(pt[0]*2)]--; //offset the right side
                leftshowed.insert(pt[0]);
            }
        }
        
        for (auto& y : m){
            for (auto& x : y.second){
                if (x.second) return false; //if count is not 0
            }
        }
        
        return true;
    }
};
```



