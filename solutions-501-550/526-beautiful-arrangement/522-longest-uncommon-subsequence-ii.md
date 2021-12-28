# 522 Longest Uncommon Subsequence II

### Problem:

Given a list of strings, you need to find the longest uncommon subsequence among them. The longest uncommon subsequence is defined as the longest subsequence of one of these strings and this subsequence should not be any subsequence of the other strings.

A subsequence is a sequence that can be derived from one sequence by deleting some characters without changing the order of the remaining elements. Trivially, any string is a subsequence of itself and an empty string is a subsequence of any string.

The input will be a list of strings, and the output needs to be the length of the longest uncommon subsequence. If the longest uncommon subsequence doesn't exist, return -1.

Example 1:

```
Input: "aba", "cdc", "eae"
Output: 3
```

Note:

1. All the given strings' lengths will not exceed 10.
2. The length of the given list will be in the range of \[2, 50\].

### Solutions:

```java
First of all, you have to understand uncommon subsequences. The question takes me a while to realize it. Indeed, 
there're many tricky solutions with one line codes by comparison with lengths of two strings. But Why? If you've
 got it already, you can directly neglect the next paragraph.

Given that A is a subsequence of B. B can be as A by deleting some characters in order. for example, cd is a 
subsequence of caeedc. As a result of deleting aee and last c, you can get a string cd. According to the definition of uncommon subsequence, it is defined as the subsequence of one of these strings and this subsequence should not be any subsequence. Therefore, dcc is an uncommon subsequence of caeedc.That is to say, you cannot alter caeedc into dcc by deleting characters in order.

Let's go back to LUS-One first. You want to find out the longest uncommon subsequence between two strings. Given that A and B. If A is a subsequence of B, and B is a subsequence of A. The answer would be -1, because A is not an uncommon subsequence of B, and B is not the uncommon subsequence of A neither. If A is an uncommon subsequence of B, the length of the uncommon subsequence is the length of A, and vice versa, you can get the length of B when B is an uncommon subsequence of A. Therefore, you can get one line code in C++.

return a==b ? -1 : max(a.size(), b.size());
Okay, Let's move toward LUS-Two. Based on I (One) question, you have to compare with all strings mutually in II (Two). First, we can define the LCS function withO(n), n depicts max(a.length(), b.length()) .

bool LCS(const string& a, const string& b) {
	if (b.size() < a.size()) return false;
	int i = 0;
    for(auto ch: b) {
		if(i < a.size() && a[i] == ch) i++;
    }
    return i == a.size();
}
Then, we can compare with all strings with O(m²), m represents the size of vector<string>. We iterate all strings mutually, and if the strings are uncommon subsequence, we record the lengths. The time complexity of the solution is O(m²n), and beats 100% submissions with 8ms. The code shows as follows:

class Solution {
public:
    int findLUSlength(vector<string>& strs) {
        if (strs.empty()) return -1;
        int rst = -1;
        for(auto i = 0; i < strs.size(); ++i) {
            int j = 0;
            for(; j < strs.size(); ++j) {
                if(i==j) continue;
                if(LCS(strs[i], strs[j])) break;  // strs[j] is a subsequence of strs[i]
            }
            // strs[i] is not any subsequence of the other strings.
            if(j==strs.size()) rst = max(rst, static_cast<int>(strs[i].size()));
        }
        return rst;
    }
    // iff a is a subsequence of b
    bool LCS(const string& a, const string& b) {
        if (b.size() < a.size()) return false;
        int i = 0;
        for(auto ch: b) {
            if(i < a.size() && a[i] == ch) i++;
        }
        return i == a.size();
    }
};
This is not a perfect solution right now, but it is quite easy to understand, even though the question gets plenty of negative votes not to be worth solving.
```

```java

```



