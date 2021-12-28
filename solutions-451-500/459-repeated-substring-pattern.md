# 459. Repeated Substring Pattern

### Problem:

Given a non-empty string check if it can be constructed by taking a substring of it and appending multiple copies of the substring together. You may assume the given string consists of lowercase English letters only and its length will not exceed 10000.

Example 1:

```
Input: "abab"

Output: True

Explanation: It's the substring "ab" twice.
```

Example 2:

```
Input: "aba"

Output: False
```

Example 3:

```
Input: "abcabcabcabc"

Output: True

Explanation: It's the substring "abc" four times. (And the substring "abcabc" twice.)
```

### Solutions:

```java
To Clarify Let's take an example of "abc" repeated 5 times. String : "abcabcabcabcabc"
The the table below

first row is the index
second row is the character corresponding to each index
third row is the value that the KMP prefix array would have at that position. (with slight variation on the index, you can build KMP in different ways)
0	1	2	3	4	5	6	7	8	9	10	11	12	13	14	15
a	b	c	a	b	c	a	b	c	a	b	c	a	b	c	
0	0	0	0	1	2	3	4	5	6	7	8	9	10	11	12
Here dp[n] = 12
n - dp[n] = 3 which is the length of the repeated substring. Ans is : (12 && (12 % 3) == 0) ===> 1

dp[n] && dp[n] % (n - dp[n]) == 0 means that the string should have some repetition (dp[n] > 0) , and the string should be divisible by the repeated substring.


First, we build the KMP table.

Roughly speaking, dp[i+1] stores the maximum number of characters that the string is repeating itself up to position i.
Therefore, if a string repeats a length 5 substring 4 times, then the last entry would be of value 15.
To check if the string is repeating itself, we just need the last entry to be non-zero and str.size() to divide (str.size()-last entry).
    bool repeatedSubstringPattern(string str) {
        int i = 1, j = 0, n = str.size();
        vector<int> dp(n+1,0);
        while( i < str.size() ){
            if( str[i] == str[j] ) dp[++i]=++j;
            else if( j == 0 ) i++;
            else j = dp[j];
        }
        return dp[n]&&dp[n]%(n-dp[n])==0;
    }
```

```java
public class Solution {
    public boolean repeatedSubstringPattern(String str) {
        int n = str.length();
        for (int count = n / 2; count >= 1; count --) {
            if (n % count == 0) {
                int num = n / count;
                StringBuilder sb = new StringBuilder();
                String cand = str.substring(0, count);
                for (int j = 0; j < num; j ++) {
                    sb.append(cand); 
                }
                if (sb.toString().equals(str)) 
                    return true;
            }
        }
        return false;
    }
}
```

```java
public class Solution {
    public boolean repeatedSubstringPattern(String str) {
        for (int i = 1; i <= str.length() / 2; i ++) {
            if (str.length() % i == 0) {
                if (str.substring(0, str.length() - i).equals(str.substring(i))) {
                    return true;
                }
            }
        }
        return false;
    }
}
```

```java
public class Solution {
    public boolean repeatedSubstringPattern(String s) {
        String doubled = s + s;
        int cut = doubled.indexOf(s, 1);
        if (cut >= 1 && cut < s.length()) {
            return true;
        }
        return false;
    }
}
```



