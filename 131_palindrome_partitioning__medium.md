# 131 Palindrome Partitioning – Medium

### Problem:

Given a string s, partition s such that every substring of the partition is a palindrome.

Return all possible palindrome partitioning of s.

For example, given s = "aab",  
Return

\[

\[“aa”,”b”\],

\["a","a","b"\]  
\]

### Thoughts:

What is a Palindrome? A palindrome is like a string “aba” where it’s the same no matter if you read from left to right or right to left.

This problem has a optimal structure. If we cut a string s into s1 and s2. Now we need to find out a palindrome partitioning both for s1 and s2. These are new subproblems.

The first idea would be divide and conquer. However, we need to check all the partitioning which means that we cannot simply cut at a point in the string and divide the problem.

So dynamic programming might be a good try.

The idea is like the Rod Cut Problem in book . Suppose string s has length n. We have i from 1 to n, check if substring 0 to i in string s is a palindrome. If not we don’t need to do more, if yes we add this substring into the solution space of partitioning problem for substring i+1 to n. After checking all the possible position, we will get the answer.

One more thing is that we need to quickly check if a certain substring in string s is a palindrome. In order to do so, we build a auxiliary array isPa to keep in mind if that certain substring is palindrome. To calculate this array, we do another dynamic programming.

### Solutions:

```cpp
class Solution {
public:
    vector<vector<string>> result;
    vector<string> currentList;
    vector<vector<string>> partition(string s) {
       
        dfs(result, s, 0, currentList);
        return result;
    }

    void dfs(string &s, int start) {
        if (start >= s.length()) result.push_back(currentList);
        for (int end = start; end < s.length(); end++) {
            if (isPalindrome(s, start, end)) {
                // add current substring in the currentList
                currentList.push_back(s.substr(start, end - start + 1));
                dfs(result, s, end + 1, currentList);
                // backtrack and remove the current substring from currentList
                currentList.pop_back();
            }
        }
    }

    bool isPalindrome(string &s, int low, int high) {
        while (low < high) {
            if (s[low++] != s[high--]) return false;
        }
        return true;
    }
};
```



