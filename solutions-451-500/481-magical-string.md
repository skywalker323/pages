# 481. Magical String

### Problem:

A magical string S consists of only '1' and '2' and obeys the following rules:

The string S is magical because concatenating the number of contiguous occurrences of characters '1' and '2' generates the string S itself.

The first few elements of string S is the following: S = "1221121221221121122……"

If we group the consecutive '1's and '2's in S, it will be:

1 22 11 2 1 22 1 22 11 2 11 22 ......

and the occurrences of '1's or '2's in each group are:

1 2    2 1 1 2 1 2 2 1 2 2 ......

You can see that the occurrence sequence above is the S itself.

Given an integer N as input, return the number of '1's in the first N number in the magical string S.

Note: N will not exceed 100,000.

Example 1:

```
Input: 6
Output: 3
Explanation: The first 6 elements of magical string S is "12211" and it contains three 1's, so return 3.
```

### Solutions:

```java
Algorithm:
Starting from prefix "122" of magical string S, we use a queue<int> q to store its digits from the second 2
(i.e., S[2]).

Initialize q with a single 2 representing the second 2 of S.
Scan and pop the front of q and push q.front() many of d's into q, where both q.front() and d are 2 and 1 and 
they alternate each time when scanned.
Repeat Step 2 until the max desired length L == n of the magical string is scanned. Meanwhile, also update digit
 1's count ones whenever d == 1.
Note:

d ^= 3 flips d between 1 and 2. You can also use d = 3-d as well.
The outer loop has size n-3 and inner loop has max size 2, so O(n) for time complexity.
  int magicalString(int n) {
      int ones = 1; queue<int> q({2});
      for (int L = 3, d = 1; L < n; d ^= 3, q.pop())
        for (int i = 0; i++ < q.front() && L++ < n; q.push(d), ones += d%2) ;

      return n? ones : 0;
  }
```

```java
int magicalString(int n) {
        // We can increase the magical string by looking at the last character as well as the groups 
        // that are examined so far. For example, we start from index 0 ('1'), then we need to find
        // the next character. It has to be different from '1' so it will be '2', and the string becomes
        // "12", hence the next group must have 2 '2's, hence the string will be "122".
        // Following the steps, each time we use the character at index called 'group' to determine which 
        // characters (1 or 2) to append to the magical string. There are a few scenarios and it is specified
        // in the code below.
        if (n <= 1)
        {
            return n == 1 ? 1 : 0; // base case
        }
        
        int res = 1, group = 1; // result, the index called group
        string s{"1"}; // initial magical string
        while (s.size() < n) // when the size of magical string < n
        {
            char c = s[s.size() - 1]; // the last character of magical string
            if (c == '1' && (s.size() <= group || s[group] == '2'))
            {
                s += "22"; // the appended character must be different from the last character
            }
            else if (c == '2' && (s.size() <= group || s[group] == '1'))
            {
                s += "1"; // if s[group] is '1' or empty, we must append only 1 '1' 
                ++res; // update the result
            }
            else if (c == '1' && s[group] == '1')
            {
                s += "2"; // s[group] is the number of '2's to be appended
            }
            else
            {
                s += "11"; // for all other cases, append '11' and update the result
                res += s.size() > n ? 1 : 2;
            }
            
            ++group;
        }
        
        return res;
    }
```

```java
public class Solution {
    public int magicalString(int n) {
        if (n <= 0) {
            return 0;
        }
        if (n <= 3) {
            return 1;
        }
        StringBuilder sb = new StringBuilder("122");
        int i = 2;
        while (sb.length() < n) {
            int num = (sb.charAt(sb.length() - 1) - '0') ^ 3;
            int repeat = sb.charAt(i) - '0';
            for (int j = 0; j < repeat; j ++) {
                sb.append(num);
            }
            i ++;
        }
        int count = 0;
        for (i = 0; i < n; i ++) {
            if (sb.charAt(i) == '1') {
                count ++;
            }
        }
        return count;
    }
}
```



