# 306 Additive Number

### Problem:

<pre>
Additive number is a string whose digits can form additive sequence.

A valid additive sequence should contain at least three numbers. Except for the first two numbers, each subsequent number in the sequence must be the sum of the preceding two.

For example:
"112358" is an additive number because the digits can form an additive sequence: 1, 1, 2, 3, 5, 8.

1 + 1 = 2, 1 + 2 = 3, 2 + 3 = 5, 3 + 5 = 8
"199100199" is also an additive number, the additive sequence is: 1, 99, 100, 199.
1 + 99 = 100, 99 + 100 = 199
Note: Numbers in the additive sequence cannot have leading zeros, so sequence 1, 2, 03 or 1, 02, 3 is invalid.

Given a string containing only digits '0'-'9', write a function to determine if it's an additive number.

Follow up:
How would you handle overflow for very large input integers?
</pre>

### Solutions:

```java
First let's do some analysis:

backtracking is quite direct choice here, select different lengths for the preceding two numbers and the third number but there are some conditions should be taken into account;
the sum of preceding two numbers should be equal to the third number, so we have to clearly specify the three numbers in the string; simply we can use a index pos to point to the start of the three numbers, the length of the first and second number then the third number will be started at the index pos+len1+len2, if this checking num3 == num1+num3 passes, then we should move further and now the first number should be the previous second number and the second number should be the previous third number and the starting pos should be pos+len1;
the termination condition will be that the starting position of the third number is the end of the string but since we at least should check three numbers, so if encountering the termination while the starting position of the current first number is 0 then return false;
as the problem indicates, no leading zero; so if the length of the first and second number is larger than 1 and if the first character of the number is zero, then directly we should return false;
overflow is another issue here, preceding numbers' upper limit can be INT_MAX but the third should be loose comparably; so we have to take advantage of atol instead of atoi here to convert string to long integer.
Improvements
Actually we don't need to check each possibility for the numbers, the following are some cases for us to accelerate the checking.

the length of first number should be less then len/2 since the third is the sum of the first two; the length of the second number should also follow the rule;
as for the third number, as soon as the third number is bigger than the sum of the first two we can just directly return false to terminate further checking;
just as analysis presented, we can take advantage of the leading zero to accelerate the third number checking process; once the length of the third number is bigger than 1 and there is a leading zero, then just return false to terminate further checking in third number.
another consideration is the starting length of the third number, since it's the sum of the previous two numbers, so the length of it should be at least the maximal of the previous two, which also accelerate the checking process.
The whole solution in C++ lies below.

class Solution {
private:
    bool check(const string& s, int pos, int len1, int len2)
    {
        int start = pos+len1+len2;
        if(start == s.length()) { if(pos==0) return false; return true; }
        int minLen = max(len1, len2);
        if((s[pos]=='0'&&len1>1) || (s[pos+len1]=='0'&&len2>1)) return false;
        long num1 = atol(s.substr(pos, len1).c_str()), num2 = atol(s.substr(pos+len1, len2).c_str());
        if(num1>INT_MAX || num2>INT_MAX) return false;
        for(int l = minLen; l <= (int)s.length()-start; ++l)
        {
            if(l>1 && s[start]=='0') return false;
            long num3 = atol(s.substr(start, l).c_str());
            if(num3 > num1+num2) return false;
            if(num3==num1+num2 && check(s, pos+len1, len2, l)) return true; 
        }
        return false;
    }
public:
    bool isAdditiveNumber(string s) 
    {
        int len = s.length();
        for(int l1 = 1; l1 <= len/2; ++l1)
            for(int l2 = 1; l2 < len-max(l1, l2); ++l2)
                if(check(s, 0, l1, l2)) return true;
        return false;
    }
};
```