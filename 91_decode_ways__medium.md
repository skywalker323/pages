# 91 Decode Ways – Medium

### Problem:

A message containing letters from A-Z is being encoded to numbers using the following mapping:

'A' -&gt; 1  
'B' -&gt; 2  
...  
'Z' -&gt; 26  
Given an encoded message containing digits, determine the total number of ways to decode it.

For example,  
Given encoded message "12", it could be decoded as "AB" \(1 2\) or "L" \(12\).

The number of ways decoding "12" is 2.

### Thoughts:

Basic dynamic programming problem. It’s very tedious with the basic and edge cases. Otherwise, it’s very straight forward.

### Solutions:

```java
int recursiveWithMemo(int index, string& str) {
        // Have we already seen this substring?
        if (memo.find(index) != memo.end()) {
            return memo[index];
        }

        // If you reach the end of the string
        // Return 1 for success.
        if (index == str.length())             return 1;
        
        // If the string starts with a zero, it can't be decoded
        if (str[index] == '0')             return 0;

        if (index == str.length() - 1)             return 1;
        
        int ans = recursiveWithMemo(index + 1, str);
        if (stoi(str.substr(index, 2)) <= 26) 
            ans += recursiveWithMemo(index + 2, str);
        // Save for memoization
        return memo[index] = ans;
    }
```



