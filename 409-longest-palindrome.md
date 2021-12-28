# 409. Longest Palindrome

### Problem:

Given a string which consists of lowercase or uppercase letters, find the length of the longest palindromes that can be built with those letters.

This is case sensitive, for example "Aa" is not considered a palindrome here.

Note:  
Assume the length of given string will not exceed 1,010.

Example:

```
Input:
"abccccdd"

Output:
7

Explanation:
One longest palindrome that can be built is "dccaccd", whose length is 7.
```

### Solutions:

```java
First, characters are counted. Even occurring characters (v[i]%2 == 0) can always be used to build a palindrome. For every odd occurring character (v[i]%2 == 1), v[i]-1 characters can be used. Res is incremented if there is at least one character with odd occurrence number.

    int longestPalindrome(string s) {
        vector<int> v(256,0);
        for(int i = 0; i < s.size(); ++i)
           ++v[s[i]];
        int res = 0;
        bool odd = false;
        for(int i = 0; i < 256; ++i)
           if(v[i]%2 == 0)
               res += v[i];
            else
            {
               res += v[i] - 1;
               odd = true;
            }
        if(odd)
          ++res;
        return res;
    }
```

```java
Same idea! Actually this is a very intuitive solution. For those who have difficulty understanding, please look at this explanation. Suppose a character appears k times. Since we want to build the palindrome string, so any characters that satisfies k is even should definitely be included. For characters that appear odd times, we can only use (k -1) times of this character to avoid non-palindrome. Only for one of them, we can use k times. (because we can make the string's length become odd by adding one character in the center of the string).

Here is an example: if we have "aabbbcccdd", then we can build "dacbcbcad".

Here is my java version code.

class Solution {
    public int longestPalindrome(String s) {
        if (s == null || s.length() == 0) {
            return 0;
        }
        int[] map = new int[58]; /* According to ASCII, there are 58 symbols from 'A' to 'z' */
        int res = 0;
        boolean hasOdd = false;
        for (char c : s.toCharArray()) { 
            map[c-'A']++;
        }
        for (int i = 0; i < 58; i++) {
            if (map[i] % 2 == 0) {
                res += map[i];
            }
            else {
                hasOdd = true;
                res += (map[i] - 1);
            }
        }
        if (hasOdd) { /* If we have encountered characters appearing odd times, add 1 to the result */
            res++;
        }
        return res;
    }
}
```



