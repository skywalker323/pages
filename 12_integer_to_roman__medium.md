# 12 Integer to Roman – Medium

### Problem:

Given an integer, convert it to a roman numeral.

Input is guaranteed to be within the range from 1 to 3999.

### Thoughts:

Key idea is to switch number by different cases.

Instead of having the base character into  
{“M”,”CM”,”D”,”CD”,”C”,”XC”,”L”,”XL”,”X”,”IX”, “V”, “IV”, “I”}  
its makes us easier to handle the special case for 9 and 4.  
By using the new base character, roman numeric can be only the 1 – 3 appearance of the characters above. So there is no special case we need to handle like we do in the above solution.  
The key takeaway here is that if a problem is very annoying and has many special case we need to handle. Think about is there a way we can have a helper variable that can help reduce the annoying part. Especially when you have a lot of if else.

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



