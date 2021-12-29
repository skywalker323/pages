# 354 Russian Doll Envelopes

### Problem:

You have a number of envelopes with widths and heights given as a pair of integers \(w, h\). One envelope can fit into another if and only if both the width and height of one envelope is greater than the width and height of the other envelope.

What is the maximum number of envelopes can you Russian doll? \(put one inside other\)

Example:  
Given envelopes = \[\[5,4\],\[6,4\],\[6,7\],\[2,3\]\], the maximum number of envelopes you can Russian doll is 3 \(\[2,3\] =&gt; \[5,4\] =&gt; \[6,7\]\).

### Solutions:

```java
class Solution {
public:
    int maxEnvelopes(vector<vector<int>>& envelopes) {
        int n=envelopes.size();
        sort(envelopes.begin(),envelopes.end());//Trick cuz the order is not same in ans;
        if(n==0)return 0;
        vector<int> dp(n+1,1);
        for(int i=0;i<n;i++){
            for(int j=0;j<i;j++){
                if(envelopes[i][0]>envelopes[j][0] && envelopes[i][1]>envelopes[j][1]){
                    dp[i]=max(dp[i],1+dp[j]);
                }
            }
        }
        int ans=0;
        for(int i=0;i<n;i++){
            ans=max(ans,dp[i]);
        }
        return ans;
    }
};
```



