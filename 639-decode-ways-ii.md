# 639 Decode Ways II

### Problem

A message containing letters from A-Z is being encoded to numbers using the following mapping way:

```
'A' -> 1
'B' -> 2
...
'Z' -> 26
```

Beyond that, now the encoded string can also contain the character '\*', which can be treated as one of the numbers from 1 to 9.

Given the encoded message containing digits and the character '\*', return the total number of ways to decode it.

Also, since the answer may be very large, you should return the output mod 109 + 7.

Example 1:

```
Input: "*"
Output: 9
Explanation: The encoded message can be decoded to the string: "A", "B", "C", "D", "E", "F", "G", "H", "I".
```

Example 2:

```
Input: "1*"
Output: 9 + 9 = 18
```

Note:  
The length of the input string will fit in range \[1, 105\].  
The input string will only contain the character '\*' and digits '0' - '9'.

### Solutions

```java
class Solution {
    private static final int M = 1000000007;
    // function of addition with mod
    private long add(long num1, long num2) {
        return (num1 % M + num2 % M) % M;
    }
    // how many ways to decode using one char
    private long ways(char a) {
        return (a == '*') ? 9 : (a != '0') ? 1 : 0; // simple way to write if else statement
    }
    // how many ways to decode using two chars
    private long ways(char a, char b) {
        if (a == '*' && b == '*') { // "**" neither is digit
            return 15;
        } else if (a == '*') {      // "*D" snd is a digit
            return (b > '6') ? 1 : 2;
        } else if (b == '*') {      // "D*" fst is a digit
            return (a == '1') ? 9 : (a == '2') ? 6 : 0;
        } else {                    // "DD" both r digits
            int val = Integer.valueOf("" + a + b);
            return (10 <= val && val <= 26) ? 1 : 0;
        }
    } 
    
    public int numDecodings(String s) {
        long[] dp = new long[s.length() + 1]; // use long to prevent overflow
        dp[0] = 1;
        dp[1] = ways(s.charAt(0));
        for (int i = 1; i < s.length(); i++) { // off-by-one error, notice s.charAt(i)'s result is stored in dp[i + 1]
            long oneCharWays = ways(s.charAt(i)) * dp[i];
            long twoCharWays = ways(s.charAt(i - 1), s.charAt(i)) * dp[i - 1];
            dp[i + 1] = add(oneCharWays, twoCharWays);
        }
        return (int)dp[s.length()]; // cast back
    }
}
```



