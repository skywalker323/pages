# 363. Max Sum of Rectangle No Larger Than K

### Problem:

Given a non-empty 2D matrix matrix and an integer k, find the max sum of a rectangle in the matrix such that its sum is no larger than k.

Example:

```
Given matrix = [
  [1,  0, 1],
  [0, -2, 3]
]
k = 2
```

The answer is 2. Because the sum of rectangle \[\[0, 1\], \[-2, 3\]\] is 2 and 2 is the max number no larger than k \(k = 2\).

Note:  
1. The rectangle inside the matrix must have an area &gt; 0.  
2. What if the number of rows is much larger than the number of columns?

### Solutions:

```java
class Solution {
public:
    int kadane(vector<int> a,int k){
        int ans=INT_MIN,cs=0,p=0;
        set<int> st;
        st.insert(0);
        for(int i=0;i<a.size();i++){
            cs+=a[i];
            if((cs-p)<=k)
                ans=max(ans,cs-p);
            else{
                auto it=st.lower_bound(cs-k);
                if(it!=st.end())
                    ans=max(ans,cs-*it);
            }
            st.insert(cs);
            p=min(p,cs);
        }
        return ans;
    }
    
    int maxSumSubmatrix(vector<vector<int>>& a, int k) {
        int ans=INT_MIN;
        for(int i=0;i<a.size();i++){
            vector<int> v(a[0].size(),0);
            for(int j=i;j<a.size();j++){
                for(int k=0;k<a[0].size();k++)
                    v[k]+=a[j][k];
                ans=max(ans,kadane(v,k));
            }
        }
        return ans;
    }
};
```

### Solutions:

```java
class Solution{
public:
    int maxSumSubmatrix(vector<vector<int>>& matrix, int k)
    {
        int m = matrix.size(), n = matrix[0].size();
        int sum[m][n], res=INT_MIN;  
        vector<int> row(m, 0);
        for(int l=0;l<n;l++)
        {
            fill(row.begin(), row.end(), 0); //refill vector with 0 with every change in pointer 'l'
            for(int r=l;r<n;r++)
            {
                int csum=0, mx=INT_MIN;
                for(int i=0;i<m;i++)
                {
                    row[i]+=matrix[i][r];
                    if(csum<0) csum = row[i];
                    else csum=csum+row[i];
                    mx=max(mx, csum);
                }
                if(mx<=k)
                {
                    res=max(res, mx); 
                    continue;
                }
                set<int> s; //if maximum sum for a l is greater than k
                s.insert(0); 
                csum=0;
                for(auto x: row)
                {
                    csum+=x;
                    auto f = s.lower_bound(csum-k); //find element which is equal to or just greater than difference between current sum and k
                    if(f!=s.end()) res=max(res, csum -*f);
                    s.insert(csum);
                }
            }
        }
        return res;
    }
};
```



