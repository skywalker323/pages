# 118 Pascal’s Triangle – Easy

### Problem:

Given numRows, generate the first numRows of Pascal’s triangle.

For example, given numRows = 5,  
Return

\[  
     \[1\],  
    \[1,1\],  
   \[1,2,1\],  
  \[1,3,3,1\],  
 \[1,4,6,4,1\]  
\]

### Thoughts:

Very straight forward based on formula of Pascal’s Triangle.

### Solutions:

```java
 vector<vector<int>> generate(int numRows) {
        if( numRows ==1) return  {{1}};
        vector<vector<int>> res{{1}, {1, 1}};
        
        for(int i =3; i <= numRows; i++)
        {
            vector<int> r = res[i-2];
            vector<int> t;
            t.push_back(1);
            for(auto j=0; j < r.size()-1; j++) t.push_back(r[j] + r[j+1]);
            t.push_back(1);
            res.push_back(t);
        }
    
       return res;        
    }
```



