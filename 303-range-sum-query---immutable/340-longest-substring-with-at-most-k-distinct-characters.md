# 340. Longest Substring with At Most K Distinct Characters

### Problem:

Given a string, find the length of the longest substring T that contains at most k distinct characters.

For example, Given s = “eceba” and k = 2,

T is "ece" which its length is 3.

### Solutions:

```java
class Solution {
public:
    int lengthOfLongestSubstringKDistinct(string s, int k) {
        if (k == 0)
            return 0;
        int charCount[128];
        memset(charCount, 0, sizeof(charCount));
        int count = 0, maxLen = 0, left = 0;
        for (int right = 0; right < s.length(); right++) {
            if (++charCount[s[right]] == 1) {
                count++;
            }
            while (count > k && left <= right) {
                if (--charCount[s[left++]] == 0) 
                    count--;
            }
            maxLen = max(maxLen, right - left + 1);
        }
        return maxLen;
    }
};
```



