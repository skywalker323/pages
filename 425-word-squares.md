# 425. Word Squares

### Problem:

Given a set of words \(without duplicates\), find all word squares you can build from them.

A sequence of words forms a valid word square if the kth row and column read the exact same string, where 0 ≤ k &lt; max\(numRows, numColumns\).

For example, the word sequence \["ball","area","lead","lady"\] forms a word square because each word reads the same both horizontally and vertically.

```
b a l l
a r e a
l e a d
l a d y
```

Note:  
1. There are at least 1 and at most 1000 words.  
2. All words will have the exact same length.  
3. Word length is at least 1 and at most 5.  
4. Each word contains only lowercase English alphabet a-z.  
Example 1:

```
Input:
["area","lead","wall","lady","ball"]

Output:
[
  [ "wall",
    "area",
    "lead",
    "lady"
  ],
  [ "ball",
    "area",
    "lead",
    "lady"
  ]
]

Explanation:
The output consists of two word squares. The order of output does not matter (just the order of words in each word square matters).
```

Example 2:

```
Input:
["abat","baba","atan","atal"]

Output:
[
  [ "baba",
    "abat",
    "baba",
    "atan"
  ],
  [ "baba",
    "abat",
    "baba",
    "atal"
  ]
]

Explanation:
The output consists of two word squares. The order of output does not matter (just the order of words in each word square matters).
```

### Solutions:

```java
idea:

Let's say we have words = ["abc", "adb", "bca", "cac"]
One straightforward method is to enumerate all permutations of words and check each square. But it's apparently too slow (for larger input size).
As we don't have any word starting with 'd', we know that "adb" can't be in the first row of a valid word square.
How can we check this in our code? instead of only adding rows to our current square, we also add columns since a valid word square has to be symmetric.
that is, after adding the word "adb" to an empty square, our square is like ["adb", "d__", "b__"] instead of ["adb", "___", "___"]
we then need to check whether there are words prefixed by d and b, if none of the words are prefixed by d (or b), which is our case, we immediately know that the current square won't be valid, no matter how we choose words afterwards. In this case, we backtrack.
Then, when we choose a word w to be the next row, say row i, in our square, we check whether the current string in square[i] is a prefix of w.
Since the words' length are at most 5, bruteforce each pair of w, square[i] are acceptable. But with precomputed hash map we can do it faster.
Method 1: Trie: 368 ms (16.08%), 14.2 MB (95.69%)

When it comes to prefix of a set of words, it is intuitive to make use of trie
code:
class TrieNode {
public:
	TrieNode* child[26];
	bool isWord;
	TrieNode(): isWord(false) {
		for (int i = 0; i < 26; i++) {
			child[i] = nullptr;
		}
	}
};

class Trie {
public:
	TrieNode root;
	Trie() {}
	void addWord(string& s) {
		TrieNode* curr = &root;
		for (char c : s) {
			if (!curr->child[c-'a']) {
				curr->child[c-'a'] = new TrieNode();
			}
			curr = curr->child[c-'a'];
		}
		curr->isWord = true;
	}
	bool hasPrefix(string& s) {
		TrieNode* curr = &root;
		for (char c : s) {
			if (c == ' ') return true;
			if (!curr->child[c-'a']) {
				return false;
			}
			curr = curr->child[c-'a'];
		}
		return true;
	}
};

class Solution {
public:
	vector<vector<string>> wordSquares(vector<string>& words) {
		Trie t;
		for (auto& w : words) {
			t.addWord(w);
		}
		vector<vector<string>> ans;
		int len = words[0].length();
		vector<string> curr(len, string(len, ' '));
		// trie + DFS
		// state: current placed strings
		// when placing a string, check the current transpose on the trie
		search(words, t, ans, curr, 0);
		return ans;
	}
private:
	void search(vector<string>& words, Trie& t, vector<vector<string>>& ans, vector<string>& curr, int i) {
		if (i == curr.size()) {
			ans.push_back(curr);
			return;
		}
		for (auto& w : words) {
			if (match(w, curr[i])) {    // whether w can be placed at i-th row
				string ori = curr[i];
				curr[i] = w;
				bool ok = true;
				for (int j = i+1; j < curr.size() && ok; j++) { // check transpose
					curr[j][i] = w[j];
					if (!t.hasPrefix(curr[j])) {    // non-existing prefix -> can't place w here
						ok = false;
					}
				}
				if (ok) {   // keep searching
					search(words, t, ans, curr, i+1);
				}
				for (int j = i+1; j < curr.size(); j++) {   // recover the state
					curr[j][i] = ' ';
				}
				curr[i] = ori;
			}
		}
	}
	bool match(string& w, string& pre) {    // check whether pre is a prefix of w
		int n = pre.length();
		for (int i = 0; i < n; i++) {
			if (pre[i] == ' ') return true;
			if (w[i] != pre[i]) return false;
		}
		return true;
	}
};
Method 2: Hash Set: 400 ms (15.29%), 12.3 MB (99.61%)

Same idea, but now we keep track of all prefixes in a hash set (unordered_set in C++).
the space it consumes is surprisingly little.
code:
class Solution {
public:
	vector<vector<string>> wordSquares(vector<string>& words) {
		unordered_set<string> prefixSet;
		for (auto w : words) {
			add(prefixSet, w);
		}
		vector<vector<string>> ans;
		int len = words[0].length();
		vector<string> curr(len, string(len, ' '));
		// hash set + DFS
		// state: current placed strings
		// when placing a string, check the current transpose on the hash set
		search(words, prefixSet, ans, curr, 0);
		return ans;
	}
private:
	void add(unordered_set<string>& prefix, string& w) {
		int n = w.length();
		string currW = "";
		for (int i = 0; i < n; i++) {
			currW += w[i];
			prefix.insert(currW + string(n-1-i, ' '));
		}
	}
	void search(vector<string>& words, unordered_set<string>& prefixSet, vector<vector<string>>& ans, vector<string>& curr, int i) {
		if (i == curr.size()) {
			ans.push_back(curr);
			return;
		}
		for (auto& w : words) {
			if (match(w, curr[i])) {    // whether w can be placed at i-th row
				string ori = curr[i];
				curr[i] = w;
				bool ok = true;
				for (int j = i+1; j < curr.size() && ok; j++) { // check transpose
					curr[j][i] = w[j];
					if (!prefixSet.count(curr[j])) {    // non-existing prefix -> can't place w here
						ok = false;
					}
				}
				if (ok) {   // keep searching
					search(words, prefixSet, ans, curr, i+1);
				}
				for (int j = i+1; j < curr.size(); j++) {   // recover the state
					curr[j][i] = ' ';
				}
				curr[i] = ori;
			}
		}
	}
	bool match(string& w, string& pre) {    // check whether pre is a prefix of w
		int n = pre.length();
		for (int i = 0; i < n; i++) {
			if (pre[i] == ' ') return true;
			if (w[i] != pre[i]) return false;
		}
		return true;
	}
};
Method 3: Hash Map: 56 ms (91.37%), 16.2 MB (82.74%)

We can keep track of all prefixes and the possible words corresponding to each prefix at the same time using a hash map.
code:
class Solution {
public:
	vector<vector<string>> wordSquares(vector<string>& words) {
		unordered_map<string, unordered_set<string>> prefixMap;  // prefix -> set of words that has it as prefix
		for (auto w : words) {
			add(prefixMap, w);
		}
		vector<vector<string>> ans;
		int len = words[0].length();
		vector<string> curr(len, string(len, ' '));
		// prefix map + DFS
		// state: current placed strings
		search(words, prefixMap, ans, curr, 0);
		return ans;
	}
private:
	void add(unordered_map<string, unordered_set<string>>& prefix, string& w) {
		int n = w.length();
		string currW(n, ' ');
		prefix[currW].insert(w);
		for (int i = 0; i < n; i++) {
			currW[i] = w[i];
			prefix[currW].insert(w);
		}
	}
	void search(vector<string>& words, unordered_map<string, unordered_set<string>>& prefixMap, vector<vector<string>>& ans, vector<string>& curr, int i) {
		if (i == curr.size()) {
			ans.push_back(curr);
			return;
		}
		for (auto& w : prefixMap[curr[i]]) {    // only need to check the words prefixed by curr[i] 
			string ori = curr[i];
			curr[i] = w;
			bool ok = true;
			for (int j = i+1; j < curr.size() && ok; j++) { // check transpose
				curr[j][i] = w[j];
				if (!prefixMap.count(curr[j])) {    // non-existing prefix -> can't place w here
					ok = false;
				}
			}
			if (ok) {   // keep searching
				search(words, prefixMap, ans, curr, i+1);
			}
			for (int j = i+1; j < curr.size(); j++) {   // recover the state
				curr[j][i] = ' ';
			}
			curr[i] = ori;
		}
	}
};
```



