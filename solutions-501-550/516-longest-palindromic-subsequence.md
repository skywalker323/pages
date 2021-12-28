# 516 Longest Palindromic Subsequence

### Problem

Given a string s, find the longest palindromic subsequence's length in s. You may assume that the maximum length of s is 1000.

Example 1:  
Input:

```
"bbbab"
```

Output:

```
4
```

One possible longest palindromic subsequence is "bbbb".  
Example 2:  
Input:

```
"cbbd"
```

Output:

```
2
```

One possible longest palindromic subsequence is "bb".

### Solutions

```java
1. Recursion:
int longestPalindromeSubseq(string s) {
	vector<vector<int>> table(s.size(), vector<int>(s.size(), 0));
	return longestPalindrome(table, s, 0, s.size() - 1);
}

int longestPalindrome(vector<vector<int>>& table, const string& s, int left, int right) {
	if (left >= right) {
		return left == right ? 1 : 0;
	}
	if (table[left][right]) {
		return table[left][right];
	}
	table[left][right] =  (s[left] == s[right] ? 2 + longestPalindrome(table, s, left + 1, right - 1) : max(longestPalindrome(table, s, left + 1, right), longestPalindrome(table, s, left, right - 1)));
	return table[left][right];
}

2. DP
int longestPalindromeSubseq(string s) {
        // table[len][start_index]
        vector<vector<int>> table(s.size() + 1, vector<int>(s.size(), 0));
        for (int j = 0; j < s.size(); ++ j) {
            table[1][j] = 1;
        }
        for (int len = 2; len <= s.size(); ++ len) {
            for (int j = 0; j <= s.size() - len; ++ j) {
                table[len][j] = s[j] == s[j + len - 1] ? 2 + table[len - 2][j + 1] :
                    max(table[len - 1][j], table[len - 1][j + 1]);
            }
        }
        return table[s.size()][0];
    }
```



