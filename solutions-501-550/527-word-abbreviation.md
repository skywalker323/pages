# 527. Word Abbreviation

### Problem:

Given an array of n distinct non-empty strings, you need to generate minimal possible abbreviations for every word following rules below.

1. Begin with the first character and then the number of characters abbreviated, which followed by the last character.
2. If there are any conflict, that is more than one words share the same abbreviation, a longer prefix is used instead of only the first character until making the map from word to abbreviation become unique. In other words, a final abbreviation cannot map to more than one original words.
3. If the abbreviation doesn't make the word shorter, then keep it as original.

Example:

```
Input: ["like", "god", "internal", "me", "internet", "interval", "intension", "face", "intrusion"]
Output: ["l2e","god","internal","me","i6t","interval","inte4n","f2e","intr4n"]
```

Note:  
1. Both n and the length of each word will not exceed 400.  
2. The length of each word is greater than 1.  
3. The words consist of lowercase English letters only.  
4. The return answers should be in the same order as the original array.

### Solutions:

```java
The idea is to sort the dict first, by comparing 3 things:
1) a.size( ) == b.size( )
2) first and last character
3) the remaining characters
After sorting, we only need compare a certain word to its previous and next word for longest common prefix, 
in order to decide its abbreviation. n is the size of dict, and m is the average length
 of word in dict. Sorting complexity is O(mnlogn), and the procedure after sorting is O(mn).


class Solution {
public:
    vector<string> wordsAbbreviation(vector<string>& dict) {
        int n = dict.size();
        vector<string> ans = dict;
        // sort the dict
        sort(dict.begin(), dict.end(), mycompare);
        unordered_map<string, string> mp;
        // prefix is the longest common prefix between dict[i] and dict[i-1]
        int prefix = 0; 
        for (int i = 0; i < n; ++i) {
            int j = 0;
            // j is the longest prefix length between dict[i] and dict[i+1]
           // if dict[i] is last word, or the length is different, or the last character is different, j = 0;
            if (i < n-1 && dict[i].size() == dict[i+1].size() && dict[i].back() == dict[i+1].back()) {
                while (j < dict[i].size() && dict[i][j] == dict[i+1][j])
                    j++;
            }
            if (j > prefix) prefix = j;
            // build abbreviation if it is shorter than word, and put it in a map
            if (dict[i].size() > prefix+3) {
                string s = dict[i].substr(0, prefix+1)+to_string(dict[i].size()-prefix-2)+dict[i].back();
                mp[dict[i]] = s;
            }
            // update prefix to be longest prefix with previous word
            prefix = j;
        }
        for (int i = 0; i < n; ++i) {
            if (mp.count(ans[i])) ans[i] = mp[ans[i]];
        }
        return ans;
    }
private:
    static bool mycompare(string& a, string& b) {
        if (a.size() == b.size()) {
            if (a.back() < b.back()) 
                return true;
            else if (a.back() > b.back()) 
                return false;
            for (int i = 0; i < a.size()-1; ++i) {
                if (a[i] < b[i]) 
                    return true;
                else if (a[i] > b[i])
                    return false;
            }
        }
        return a.size() < b.size();
    }
};
```



