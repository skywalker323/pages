# 14 Longest Common Prefix – Easy

### Problem:

Write a function to find the longest common prefix string amongst an array of strings.

### Thoughts:

Get the first string in the array of strings, the longest common prefix is at best this first string.

So iterate over all strings, and find out if there is a common prefix for the current string and current prefix.

If the first string length is m, and there are n strings. This method will be O\(mn\) running time.

There could be a optimized solution. Instead of always start with the first string in the array, init the prefix to be the shortest string in the array. If k is the shortest string’s length, then running time would be O\(kn\). This is helpful when k is significantly smaller than m, where m is the first string length.

### Solutions:

```java
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        
        sort(begin(strs), end(strs), [](string s1, string s2) { return s1.size() < s2.size();});
        
        int  mx = INT_MAX;
        string s = strs[0];
        for(int i = 1; i< strs.size(); i++)
        {
            int j =0;
            for(; j < s.size(); j++)
            {
                if(s[j] != strs[i][j]) break;
            }
            
            mx = min(mx, j);
        }
        
        return s.substr(0, mx);
    }
};
```



