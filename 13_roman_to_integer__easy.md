# 13 Roman to Integer – Easy

### Problem:

Given a roman numeral, convert it to an integer.

Input is guaranteed to be within the range from 1 to 3999.

### Thoughts:

The most straightforward way is to calculate by imitating human behavior.

Give a roman, find the largest digit, then subtract the left part and add the right part. Do this recursively and we could get our number in the end.

This is going to be O\(nlgn\), where is the number of digits in Roman representation.

There is a better solution with only O\(n\) running time. Key idea is always add a corresponding integer number, and compare current digit if it’s larger than previous, if so, subtract double of the previous number. This is because we need to subtract previous number but we already added it to the result, so we need to double it.

### Solutions:

```java
int romanToInt(string s) 
    {
       unordered_map<char, int> um{{'I', 1}, {'V', 5}, {'X', 10}, {'L', 50}, {'C' , 100}, {'D', 500}, {'M', 1000}};

        int no =0;
        for(int i =0; i < s.size(); i++)
        {
            if( i > 0 and um[s[i-1]] < um[s[i]] )
                no -= (2*um[s[i-1]]);
            no += um[s[i]];
        }

        return no;
    }
```



