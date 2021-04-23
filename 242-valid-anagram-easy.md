# 242 Valid Anagram – Easy

### Problem:
Given two strings s and t, write a function to determine if t is an anagram of s.

For example,
s = “anagram”, t = “nagaram”, return true.
s = “rat”, t = “car”, return false.

Note:
You may assume the string contains only lowercase alphabets.

Follow up:
What if the inputs contain unicode characters? How would you adapt your solution to such case?

### Thoughts:
The most straightforward way is to covert each String into char array then sort each array. Then turn the sorted char array back to String. If two Strings are Anagram, then the sorted String should be identical. The running time will be O(Math.max(m, n)) when m and n is the length of two string. Actually, we can also add an additional check at the beginning that if (s.size()!=t.size() because if the size of two strings are different, they cannot be anagram.

With the help of two HashMap, we can improve the running time to O(n), where n is the length of the given String. Also since the given requirement says string only contains lowercase alphabets. So that we can use a int[26] to replace HashMap. If the given string has unicode, then we have to use HashMap.

### Solutions:

```java
class Solution {
public:
    bool isAnagram(string s, string t) {
        std::sort(s.begin(), s.end());
        std::sort(t.begin(), t.end());
        return s == t;
    }
};
```
