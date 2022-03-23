# 8 String to Integer \(atoi\) – Easy

### Problem:

Implement atoi to convert a string to an integer.

Notes: It is intended for this problem to be specified vaguely \(ie, no given input specs\). You are responsible to gather all the input requirements up front.

### Thoughts:

The tricky part for this problem is how to handle overflow.  
The annoying thing here is the special cases. “010” -&gt; 10 “+-2″ -&gt; 0 ” 010″ -&gt; 10 “-012ab1234” -&gt; -12 “2147483648” -&gt; “2147483647” “-2147483648” -&gt; – 2147483648 Although I would think for some of these special cases we should throw Exceptions instead of returning things.

### Solutions:

```java
int myAtoi(string s) {
        
        if(s.empty()) return 0;
        int idx = 0;
        int n = s.size();
        while(s[idx] == ' ') idx++;
        bool sn = s[idx] == '-' ?  true: false;
        if( sn or s[idx] == '+') idx++;
        
        while(idx < n and  s[idx] == '0') idx++;
        
        long long no =0;
        while(idx < n and isdigit(s[idx]))     
        {
            // check overflow 
            no = no*10+s[idx]-'0';
            if( no >  INT_MAX)   return sn  ?  INT_MIN : INT_MAX;
            idx++;
        }
        
        if( no >  INT_MAX) 
            return sn  ?   INT_MIN : INT_MAX;
        
        return  sn ? -no : no;
    }
```

Updated: 10/17/2016

```java
public class Solution {
    public int myAtoi(String str) {
        str = str.trim();
        if (str.length() == 0){
            return 0;
        }
        int i = 0, flag = 1, plus = 0, minus = 0;
        long result = 0;
        if (str.charAt(0) == '-') {
            flag = -1;
            i = 1;
        }
        else if (str.charAt(0) == '+') {
            i = 1;
        }
        while (i < str.length()) {
            if (str.charAt(i) >= '0' && str.charAt(i) <= '9') {
                int digit = str.charAt(i) - '0';
                result = result * 10 + digit;
                if (result > Integer.MAX_VALUE) {
                    break;
                }
            }
            else {
                break;
            }
            i ++;
        }
        if (result*flag > Integer.MAX_VALUE) {
            return Integer.MAX_VALUE;
        }
        if (result*flag < Integer.MIN_VALUE) {
            return Integer.MIN_VALUE;
        }
        return (int) result*flag;
    }
}
```



