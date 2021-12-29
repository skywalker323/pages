# 680 Valid Palindrome II

### Problem

Given a non-empty string s, you may delete at most one character. Judge whether you can make it a palindrome.

Example 1:

```
Input: "aba"
Output: True
```

Example 2:

```
Input: "abca"
Output: True
Explanation: You could delete the character 'c'.
```

### Solutions

```java
Check from left and right at the same time until the first different pair.
Now we have something like a****b, where a and b are different.
We need to delete either a or b to make it a palindrome.

C++

    bool validPalindrome(string s) {
        for (int i = 0, j = s.size() - 1; i < j; i++, j--)
            if (s[i] != s[j]) {
                int i1 = i, j1 = j - 1, i2 = i + 1, j2 = j;
                while (i1 < j1 && s[i1] == s[j1]) {i1++; j1--;};
                while (i2 < j2 && s[i2] == s[j2]) {i2++; j2--;};
                return i1 >= j1 || i2 >= j2;
            }
        return true;
    }
    
    
This problem is quite huge if you try to solve using Brute force.

BRUTE FORCE
The most naive approach would be:

Remove every element once and apply isPalindrome() every time.
If any time you find that its palindrome then return true
If you reach end of sting then return false
The worst part for this type of solution is that the string length can be 50000 (see question).
So remove each element means it will run O(50000) and each isPalindrome is O(N) making it O(50000N)

OPTIMIZED
Just maintain 2 pointers i.e for start and end of string
Keep checking if they are same

If they are Same - then just check inside and keep going till you reach the center(left==right)(if odd string) or left>right (if even string)
If they are NOT same : You now have 2 options
2.1) Remove Left Element and Check for the Rest of String OR
2.2) Remove Right Element and Check for the Rest of the string.
If either of them dont give palindrome then its not a palindorme.
class Solution {
public:
    bool validPalindrome(string s) {
        int m = s.size();
        if(m<=2) return true;
        
        // Keep two pointers Left and Right
        int L,R;    
        L = 0;
        R = m-1;
        
        while(L<=R)
        {
            //Both first and last element match
            if(s[L]==s[R])
            {
                ++L;--R;
            }
            
            // One of the element is not matching
            else 
            {
                //Removing Left
                int LL = L+1, LR = R;
                while(LL<=LR && s[LL]==s[LR]) {++LL;--LR;}
                if(LL>=LR) return true;
                
                //Removing Right
                int RL = L, RR = R-1;
                while(RL<=RR && s[RL]==s[RR]) {++RL;--RR;}
                if(RL>=RR) return true;

                return false;

            }
 
        }
        
        return true; //It was already a Palindrome
    }
};
```



