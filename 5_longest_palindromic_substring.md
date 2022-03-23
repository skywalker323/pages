# 5 Longest Palindromic Substring

### Problem:

Given a string S, find the longest palindromic substring in S. You may assume that the maximum length of S is 1000, and there exists one unique longest palindromic substring.

### Thoughts:

Trivial way is to calculate each substring is palindrome, using O\(n^2\) time and space.  
Could be optimized using only O\(1\) space.  
Idea is to return the longest palindromic substring that is center by \(i,i\) or \(i, i + 1\).

### Solution:

Trivial way:

```java
string longestPalindrome(string s) {
        vector<vector<bool>> dp(s.size(), vector<bool>(s.size(), false));
        
        for(int i =1; i < s.size(); i++)
            dp[i][i]=1;
        
        int start = 0;
        int end = 0;
        for(int len = 2; len <= s.size(); len++) // abcdef
        {
            for(int i =0; i < s.size()-len+1; i++) // len 5 (0-4)=> 0, 1 , 2 , 3 
            {
                int j = i+len-1; // end of substr
                if(s[i] == s[i+len-1]) // when len =2 (0, 1) => dp[1][0] which is false so handl seperate
                    dp[i][j] = len == 2 ? true : dp[i+1][j-1]; 
                  
                if( dp[i][j]) { start = i; end = j;}                                
            }
        }
        
        return s.substr(start, end-start+1);
        
    }

```

The better way:

```java
public class Solution {
    public String longestPalindrome(String s) {
        String longest = "";
        for (int i = 0; i < s.length(); i ++){
            String tmp = "";
            tmp = paCenterBy(s, i, i);
            if (tmp.length() > longest.length()){
                longest = tmp;
            }
            tmp = paCenterBy(s, i, i + 1);
            if (tmp.length() > longest.length()){
                longest = tmp;
            }
        }  
        return longest;
    }
    private String paCenterBy(String s, int cleft, int cright){
        String result = "";
        while (cleft >=0 && cright < s.length() && s.charAt(cleft) == s.charAt(cright)){
            cleft --;
            cright ++;
        }
        return s.substring(cleft + 1, cright);
    }
}
```



