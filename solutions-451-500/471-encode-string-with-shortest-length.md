# 471. Encode String with Shortest Length

### Problem:

Given a non-empty string, encode the string such that its encoded length is the shortest.

The encoding rule is: k\[encoded\_string\], where the encoded\_string inside the square brackets is being repeated exactly k times.  
Note:  
1. k will be a positive integer and encoded string will not be empty or have extra space.  
2. You may assume that the input string contains only lowercase English letters. The string's length is at most 160.  
3. If an encoding process does not make the string shorter, then do not encode it. If there are several solutions, return any of them is fine.

Example 1:

```
Input: "aaa"
Output: "aaa"
Explanation: There is no way to encode it such that it is shorter than the input string, so we do not encode it.
```

Example 2:

```
Input: "aaaaa"
Output: "5[a]"
Explanation: "5[a]" is shorter than "aaaaa" by 1 character.
```

Example 3:

```
Input: "aaaaaaaaaa"
Output: "10[a]"
Explanation: "a9[a]" or "9[a]a" are also valid solutions, both of them have the same length = 5, which is the same as "10[a]".
```

Example 4:

```
Input: "aabcaabcd"
Output: "2[aabc]d"
Explanation: "aabc" occurs twice, so one answer can be "2[aabc]d".
```

Example 5:

```
Input: "abbbabbbcabbbabbbc"
Output: "2[2[abbb]c]"
Explanation: "abbbabbbc" occurs twice, but "abbbabbbc" can also be encoded to "2[abbb]c", so one answer can be "2[2[abbb]c]".
```

### Solutions:

```java
public class Solution {
    public String encode(String s) {
        String[][] dp = new String[s.length()][s.length()];
        for (int l = 1; l <= s.length(); l ++) {
            for (int i = 0; i + l <= s.length(); i ++) {
                int j = i + l - 1;
                dp[i][j] = s.substring(i, j + 1);
                for (int k = i; k < j ; k ++) {
                    String cand = dp[i][k] + dp[k + 1][j];
                    if (cand.length() < dp[i][j].length()) {
                        dp[i][j] = cand;
                    }
                }
                String range = s.substring(i, j + 1);
                String drange = range + range;
                int cut = drange.indexOf(range, 1);
                if (cut != -1 && cut < range.length()) {
                    String cand = range.length() / cut + "[" + dp[i][i + cut - 1] +"]"; 
                    if (cand.length() < dp[i][j].length()) {
                        dp[i][j] = cand;
                    }
                }
            }
        }
        String res = dp[0][dp.length - 1];
        return res;
    }
}



This is my version of the popular DP solution many have posted here. Comments and key observations are added below.

The shortest encoded string of any string s must be one of the following three cases, whichever is the shortest:

string s itself: "aaa";
repetition of a substring: "ababababab" -> "5[ab]";
concatenation of its two substrings' shortest encoded strings: "aaaaabbbbbb" -> "5[a]6[b]".
So the DP idea will be straightforward to bottom up calculate each substring's shortest encoded string as well as
 their repetitive substrings, and compare lengths to get the optimal results.

Note that shorter substrings' encoded strings must be calculated before longer ones because of case 3.

I have a post here if you are interested to know the proof behind the implementation of method collapse to calculate
 the shortest length of a string's repetitive substring.

    string encode(string& s) {
	dp = vector<vector<string>>(n, vector<string>(1+(n = s.size())));
	for (L = 1; L <= n; ++L)
          for (i = 0; i + L - 1 < n; ++i) {
    		collapse(dp[i][L] = s.substr(i,L));
    		for (int j = 1; j < L; ++j)
    		  if (dp[i][j].size()+dp[i+j][L-j].size() < dp[i][L].size()) dp[i][L] = dp[i][j]+dp[i+j][L-j];
    	  }
	return dp[0][n];        
    }
    
    vector<vector<string>> dp; // dp[i][L] = shortest encoded string of s.substr(i,L)
    size_t n, pos, i/*start index*/, L/*sub length*/;
    
    void collapse(string& s) { // collapse s if shorter
	if ((pos=(s+s).find(s,1)) < L) s = to_string(L/pos)+'['+dp[i][pos]+']';
    }
```



