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
        int len = s.length();
        // use a set to store words to avoid redundant words
		unordered_set<string> words(wordDict.begin(), wordDict.end());
		// dp[i] represents whether a substring starting at index i can be broken down using dict words
        vector<bool> dp(len+1, false);
        dp[len] = true;
		
		// outer for loop (loop over entire string from end to beginning)
        for (int i = len-1; i >= 0; --i)
		{
		    // inner for loop (loop over all possible substrings starting at index i
            for (int j = len-1; j >= i; --j)
			{
				// if the current substring matches any word and the remaining substring is valid
				// note that since we're going from end to beginning, the validity of remaining substring
				// will always already have been determined
                if (words.find(s.substr(i, j-i+1)) != words.end() && dp[j+1])
				{
                    dp[i] = true;
                    break;
                }
            }
        }
        return dp[0];
    }
```



