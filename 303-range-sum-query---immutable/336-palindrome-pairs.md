# 336. Palindrome Pairs

### Problem:

Given a list of unique words, find all pairs of distinct indices \(i, j\) in the given list, so that the concatenation of the two words, i.e. words\[i\] + words\[j\] is a palindrome.

Example 1:  
Given words = \["bat", "tab", "cat"\]  
Return \[\[0, 1\], \[1, 0\]\]  
The palindromes are \["battab", "tabbat"\]  
Example 2:  
Given words = \["abcd", "dcba", "lls", "s", "sssll"\]  
Return \[\[0, 1\], \[1, 0\], \[3, 2\], \[2, 4\]\]  
The palindromes are \["dcbaabcd", "abcddcba", "slls", "llssssll"\]

### Solutions:

```java
class Solution {
public:
    // n: words.length <= 5000, k: words[i].length <= 300, 
    // 2 options: 
    // 1) O(n*n*k)-> concatenate each word with all other, check if palindrome
    
    // 2) O(n*k*k)-> 
    // O(n): for each word, 
    // O(k): divide it into all possible left and right half
    // O(k): check if right half is palindrome 
    // and reverse of left half is present in words
    // so that on concatenation of left|right|rev(left), a palindrome is obtained
    // it accounts for all cases even when concatnated words are of unequal size
    // do the same for left as right and right as left
    
    // since n is much higher than k, will go with 2nd option.

    bool ispalin(string s) {
        int i=0, j=s.length()-1;
        while(i<j) {
            if(s[i++]!=s[j--]) { return false; }
        }
        return true;
    }
    
    vector<vector<int>> palindromePairs(vector<string>& words) {  
        vector<vector<int>> res;
        
        map<string, int> m;
        for(int i=0; i<words.size(); i++) {
            string rev = words[i];  // reversed here so that don't have to reverse every subarray of each string later
            reverse(rev.begin(), rev.end());
            m[rev] = i;
        }
        
        for(int j=0; j<words.size(); j++) {
            for(int i=0; i<words[j].size(); i++) {
                
                string left = words[j].substr(0,i);
                string right = words[j].substr(i,words[j].size()-i);
                
                if(ispalin(right) && m.find(left)!=m.end() && m[left]!=j) {
                    res.push_back(vector<int>{j, m[left]});
                
                    // left=="" means right, which has entire string, is palindrome.
                    // also left is present in map so "" string is there.
                    // So normally only word+"" is captured but ""+word isn't captured
                    // because right is never empty string to avoid duplicates (abcd, dcba: if left is abcd, we get abcddcba, if right is abcd, we get dcbaabcd; similaryly for string dcba as left and right we get the same 2 strings.)
                    // so to include case ""+word below line
                    if(left=="") { res.push_back(vector<int>{m[left], j}); }
                }
                
                if(ispalin(left) && m.find(right)!=m.end() && m[right]!=j) {
                    res.push_back(vector<int>{m[right], j});
                    if(right=="") { res.push_back(vector<int>{m[right]}); }
                }
            }
        }
            
        return res;
    }
};
```



