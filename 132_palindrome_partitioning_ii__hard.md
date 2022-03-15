# 132 Palindrome Partitioning II – Hard

### Problem:

Given a string s, partition s such that every substring of the partition is a palindrome.

Return the minimum cuts needed for a palindrome partitioning of s.

For example, given s = “aab”,  
Return 1 since the palindrome partitioning \[“aa”,”b”\] could be produced using 1 cut.

### Thoughts:

This is very similar with the first version of the problem. Palindrome Partitioning. The difference is that instead of generating all the partition, we need the minimum cut now.  
It’s fine to still generate all the partition and then count the cut of each partition.  
But there could be better way to solve this in DP.  
We still needs a helper matrix isP\[i\]\[j\] to indicate if substring i…j \(including\) is a palindrome.  
Then another one dimension array is min\[i\] indicate the minimum cut for substring 0…i \(including\).

min\[i\] = 0 when i = 1  
min\[i\] = 0 when i &gt; 1 and 0…i is a palindrome  
min\[i\] = minimum of min\[j\] + 1 where j is 1..i\(including\) when i &gt; 1 and 0…i is not a palindrome

### Solutions:

```cpp
class Solution {
public:
    vector<int> cutsDp;
    vector<vector<bool>> palindromeDp;
    
    int minCut(string s) {
        cutsDp.resize(s.length());
        palindromeDp.resize(s.length(), vector<bool>(s.length(), false));
        // build the palindrome cutsDp for all susbtrings
        buildPalindromeDp(s, s.length());
        for (int end = 0; end < s.length(); end++) {
            int minimumCut = end;
            for (int start = 0; start <= end; start++) {
                if (palindromeDp[start][end]) {
                    minimumCut =
                        start == 0 ? 0 : min(minimumCut, cutsDp[start - 1] + 1);
                }
            }
            cutsDp[end] = minimumCut;
        }
        return cutsDp[s.length() - 1];
    }

    void buildPalindromeDp(string &s, int n) {
        for (int end = 0; end < s.length(); end++) {
            for (int start = 0; start <= end; start++) {
                if (s[start] == s[end] &&
                    (end - start <= 2 || palindromeDp[start + 1][end - 1])) {
                    palindromeDp[start][end] = true;
                }
            }
        }
    }
};
```



