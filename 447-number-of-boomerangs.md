# 447 Number of Boomerangs

### Problem:

Given n points in the plane that are all pairwise distinct, a "boomerang" is a tuple of points \(i, j, k\) such that the distance between i and j equals the distance between i and k \(the order of the tuple matters\).

Find the number of boomerangs. You may assume that n will be at most 500 and coordinates of points are all in the range \[-10000, 10000\] \(inclusive\).

Example:

```
Input:
[[0,0],[1,0],[2,0]]

Output:
2

Explanation:
The two boomerangs are [[1,0],[0,0],[2,0]] and [[1,0],[2,0],[0,0]]
```

### Solutions:

```java
For each point i, map<distance d, count of all points at distance d from i>.
Given that count, choose 2 (with permutation) from it, to form a boomerang with point i.
[use long appropriately for dx, dy and key; though not required for the given test cases]

Time Complexity: O(n^2)

Updated: Using initial size for the map to avoid table resizing. Thanks @StefanPochmann

int numberOfBoomerangs(vector<pair<int, int>>& points) {
    
    int res = 0;
    
    // iterate over all the points
    for (int i = 0; i < points.size(); ++i) {
        
        unordered_map<long, int> group(points.size());
        
        // iterate over all points other than points[i]
        for (int j = 0; j < points.size(); ++j) {
            
            if (j == i) continue;
            
            int dy = points[i].second - points[j].second;
            int dx = points[i].first - points[j].first;
            
            // compute squared euclidean distance from points[i]
            int key = dy * dy;
            key += dx * dx;
            
            // accumulate # of such "j"s that are "key" distance from "i"
            ++group[key];
        }
        
        for (auto& p : group) {
            if (p.second > 1) {
                /*
                 * for all the groups of points, 
                 * number of ways to select 2 from n = 
                 * nP2 = n!/(n - 2)! = n * (n - 1)
                 */
                res += p.second * (p.second - 1);
            }
        }
    }
    
    return res;
}
```



