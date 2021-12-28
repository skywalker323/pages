# 415 Add Strings

### Problem:

Given two non-negative integers num1 and num2 represented as string, return the sum of num1 and num2.

Note:

1. The length of both num1 and num2 is &lt; 5100.
2. Both num1 and num2 contains only digits 0-9.
3. Both num1 and num2 does not contain any leading zero.
4. You must not use any built-in BigInteger library or convert the inputs to integer directly.

### Solutions:

```cpp
#define pb push_back
class Solution {
public:
    string addStrings(string num1, string num2) {
        reverse(num1.begin(), num1.end());
        reverse(num2.begin(), num2.end());
        
        if(num1.size() > num2.size())
            swap(num1, num2);
        
        string ans = "";
        int carry  = 0;
        
        for(int i = 0; i < num1.size(); i++){
            int sum = (num1[i] - '0') + (num2[i] - '0') + carry;
            ans.pb(sum % 10 + '0');
            carry = sum / 10;
        }
        
        for(int i = num1.size(); i < num2.size(); i++){
            int sum = (num2[i] - '0') + carry;
            ans.pb(sum % 10 + '0');
            carry = sum / 10;
        }
        
        if(carry != 0)
            ans.pb(carry + '0');
        
        reverse(ans.begin(), ans.end());
        
        return ans;
    }
};
```



