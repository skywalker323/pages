# 318 Maximum Product of Word Lengths

### Problem:

Given a string array words, find the maximum value of length\(word\[i\]\) \* length\(word\[j\]\) where the two words do not share common letters. You may assume that each word will contain only lower case letters. If no such two words exist, return 0.

Example 1:  
Given \["abcw", "baz", "foo", "bar", "xtfn", "abcdef"\]  
Return 16  
The two words can be "abcw", "xtfn".

Example 2:  
Given \["a", "ab", "abc", "d", "cd", "bcd", "abcd"\]  
Return 4  
The two words can be "ab", "cd".

Example 3:  
Given \["a", "aa", "aaa", "aaaa"\]  
Return 0  
No such pair of words.

### Solutions:

```cpp
class Solution {
public:
    int maxProduct(vector<string>& A) {
        int n = A.size(), ans = 0;
        vector<int> masks(n);
        for(int i = 0; i < n; i++){
            string& s = A[i];
            int S = 0;
            for(char& c: s){
                S |= 1 << (c-'a');
            }
            masks[i] = S;
        }
        for(int i = 0; i < n; i++){
            for(int j = i+1; j < n; j++){
                if((masks[i] & masks[j] )== 0) ans = max(ans, (int)A[i].size() * (int)A[j].size());
            }
        }
        return ans;
    }
};
```



