# 10 Regular Expression Matching

### Problem:

Implement regular expression matching with support for '.' and '\*'.

```
'.' Matches any single character.
'*' Matches zero or more of the preceding element.

The matching should cover the entire input string (not partial).

The function prototype should be:
bool isMatch(const char *s, const char *p)

Some examples:
isMatch("aa","a") → false
isMatch("aa","aa") → true
isMatch("aaa","aa") → false
isMatch("aa", "a*") → true
isMatch("aa", ".*") → true
isMatch("ab", ".*") → true
isMatch("aab", "c*a*b") → true
```

### Solutions:

```java
map<pair<int, int>, bool> um;
    bool match (string s, string p, int s1, int p1)
    {
        if( s1 == s.size() and p1 == p.size() ) return true;
        if( p1 == p.size() ) return false;
        
        //  "ab", p = ".*"
        // if *  normal compare
        if( um.count({s1, p1})) return um[{s1, p1}];
        if( p1 != p.size()-1 and p[p1+1] == '*') 
        {
                //.* or C* 
                if(match(s, p, s1, p1+2)) return true;
                // zero or more
                if(s1 < s.size() and (s[s1] == p[p1] || p[p1] == '.') )
                    return um[{s1, p1}] = match(s, p, s1+1, p1);
                else
                    return um[{s1, p1}] =false;
            
        }
        else
        {
             // normal case
            if(s1 < s.size() and (p[p1] == '.' || s[s1] == p[p1] ) )
                 return um[{s1, p1}] = match(s, p, s1+1, p1+1);
            else
                return um[{s1, p1}] =false;
        }
        if( s1 == s.size() and p1 == p.size()) return true;
        return um[{s1, p1}] = false;
    }
    
    bool isMatch(string s, string p) {
        return match(s, p, 0, 0);
    }
```

```java
class Solution {
    public boolean isMatch(String s, String p) {
        boolean[][] dp = new boolean[s.length() + 1][p.length() + 1];
        dp[0][0] = true;
        for (int j = 1; j < dp[0].length; j ++) {
            if (p.charAt(j - 1) == '*' && j >= 2) {
                dp[0][j] = dp[0][j - 2];
            }
        }
        for (int i = 1; i < dp.length; i ++) {
            for (int j = 1; j < dp[i].length; j ++) {
                if (p.charAt(j - 1) == '*') {
                    if (j >= 2) {
                        dp[i][j] = dp[i][j] || dp[i][j - 2];
                    }
                    if (p.charAt(j - 2) == '.' || p.charAt(j - 2) == s.charAt(i - 1)) {
                        dp[i][j] = dp[i][j] || dp[i - 1][j];
                    }

                }
                else {
                    if (p.charAt(j- 1) == '.' || p.charAt(j - 1) == s.charAt(i - 1)) {
                        dp[i][j] = dp[i - 1][j - 1];
                    }
                }
            }
        }
        return dp[s.length()][p.length()];
    }
}
```



