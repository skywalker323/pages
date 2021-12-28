# 526. Beautiful Arrangement

### Problem:

Suppose you have N integers from 1 to N. We define a beautiful arrangement as an array that is constructed by these N numbers successfully if one of the following is true for the ith position \(1 ≤ i ≤ N\) in this array:

The number at the ith position is divisible by i.  
i is divisible by the number at the ith position.  
Now given N, how many beautiful arrangements can you construct?

Example 1:

```
Input: 2
Output: 2
Explanation: 

The first beautiful arrangement is [1, 2]:

Number at the 1st position (i=1) is 1, and 1 is divisible by i (i=1).

Number at the 2nd position (i=2) is 2, and 2 is divisible by i (i=2).

The second beautiful arrangement is [2, 1]:

Number at the 1st position (i=1) is 2, and 2 is divisible by i (i=1).

Number at the 2nd position (i=2) is 1, and i (i=2) is divisible by 1.
```

Note:  
1. N is a positive integer and will not exceed 15.

### Solutions:

```java
  int countArrangement(int N) {
        int count = 0;
        std::vector<bool> visited(N);
        std::vector<int> path;
        dfs(N, path, visited, count);
        return count;
    }
    
private:
    void dfs(int N, std::vector<int>& path, std::vector<bool>& visited, int& count)
    {
        if (path.size() == N) // find a valid path
        {
            ++count; // update count
            return; // backtracking
        }
        int l = path.size() + 1; // the position to add value
        for (int i = 1; i <= N; ++i)
        {
            if ((i % l == 0 || l % i == 0) && visited[i] == false)
            {
                visited[i] = true;
                path.push_back(i);
                dfs(N, path, visited, count);
                visited[i] = false; // recover stack
                path.pop_back(); // recover stack
            }
        }
    }
```



