# 139 Word Break – Medium

### Problem:

Given a string s and a dictionary of words dict, determine if s can be segmented into a space-separated sequence of one or more dictionary words.

For example, given  
s = "leetcode",  
dict = \["leet", "code"\].

Return true because "leetcode" can be segmented as "leet code".

### Thoughts:

We could use a dynamic programing approach for this problem.

A\[i\] stands for for the substring \[0, i\], if it is a valid word break string.

A\[i\] = A\[j\] && dict.contains\(substring\(j+1, i\)\)

### Solutions:

```cpp
bool wordBreak(string s, vector<string>& wordDict) {
        unordered_set<string> wordSet(wordDict.begin(), wordDict.end());
        vector<vector<bool>> dp(s.size(), vector<bool>(s.size(), false));
        
        for (int i = 0; i < s.size(); i++){
            for (int j = i; j < s.size(); j++){
                if (wordSet.count(s.substr(i, j+1-i)) > 0){
                    if (i == 0){
                        dp[i][j] = true;
                        if (j == s.size()-1){
                            return true;
                        }
                    }
                    for (int col = 0; col < i; col++){
                        if (dp[col][i-1]){
                            dp[i][j] = true;
                            if (j == s.size()-1){
                                return true;
                            }
                            break;
                        }
                    }
                }
            }
        }
        return false;
    }
```



