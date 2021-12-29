# 642 Design Search Autocomplete System

### Problem

Design a search autocomplete system for a search engine. Users may input a sentence (at least one word and end with a special character '#'). For each character they type except '#', you need to return the top 3 historical hot sentences that have prefix the same as the part of sentence already typed. Here are the specific rules:

1. The hot degree for a sentence is defined as the number of times a user typed the exactly same sentence before.
2. The returned top 3 hot sentences should be sorted by hot degree (The first is the hottest one). If several sentences have the same degree of hot, you need to use ASCII-code order (smaller one appears first).
3. If less than 3 hot sentences exist, then just return as many as you can.
4. When the input is a special character, it means the sentence ends, and in this case, you need to return an empty list.
Your job is to implement the following functions:

The constructor function:

AutocompleteSystem(String[] sentences, int[] times): This is the constructor. The input is historical data. Sentences is a string array consists of previously typed sentences. Times is the corresponding times a sentence has been typed. Your system should record these historical data.

Now, the user wants to input a new sentence. The following function will provide the next character the user types:

List<String> input(char c): The input c is the next character typed by the user. The character will only be lower-case letters ('a' to 'z'), blank space (' ') or a special character ('#'). Also, the previously typed sentence should be recorded in your system. The output will be the top 3 historical hot sentences that have prefix the same as the part of sentence already typed.


Example:
```
Operation: AutocompleteSystem(["i love you", "island","ironman", "i love leetcode"], [5,3,2,2]) 
The system have already tracked down the following sentences and their corresponding times: 
"i love you" : 5 times 
"island" : 3 times 
"ironman" : 2 times 
"i love leetcode" : 2 times 
Now, the user begins another search: 

Operation: input('i') 
Output: ["i love you", "island","i love leetcode"] 
Explanation: 
There are four sentences that have prefix "i". Among them, "ironman" and "i love leetcode" have same hot degree. Since ' ' has ASCII code 32 and 'r' has ASCII code 114, "i love leetcode" should be in front of "ironman". Also we only need to output top 3 hot sentences, so "ironman" will be ignored. 

Operation: input(' ') 
Output: ["i love you","i love leetcode"] 
Explanation: 
There are only two sentences that have prefix "i ". 

Operation: input('a') 
Output: [] 
Explanation: 
There are no sentences that have prefix "i a". 

Operation: input('#') 
Output: [] 
Explanation: 
The user finished the input, the sentence "i a" should be saved as a historical sentence in system. And the following input will be counted as a new search. 
```

Note:
The input sentence will always start with a letter and end with '#', and only one blank space will exist between two words.
The number of complete sentences that to be searched won't exceed 100. The length of each sentence including those in the historical data won't exceed 100.
Please use double-quote instead of single-quote when you write test cases even for a character input.
Please remember to RESET your class variables declared in class AutocompleteSystem, as static/class variables are persisted across multiple test cases. Please see here for more details.


### Solutions

```java
class AutocompleteSystem {
public:
    typedef pair<int,const char*> hotstr;
    unordered_map<string,int> hotmap;
    vector<string>::iterator l, r;
    vector<string> sentences;
    string userString;
    
    AutocompleteSystem(vector<string>& sentences_, vector<int>& times) {
        hotmap.clear();
        userString = "";
        sentences = sentences_;
        for (int i = 0; i < sentences.size(); ++i)
            hotmap[sentences[i]] = times[i];
        sort(sentences.begin(), sentences.end());
    }
    
    vector<string> input(char c) {
        if (c == '#') {
            insert(userString);
            userString = "";
            return {};
        } else if (userString.length() == 0) {
            l = sentences.begin();
            r = sentences.end();
        }
        userString += c;
        
        // Get iterator to range start that matches userString (lower_bound)
        // lower_bound: Iterator to the first element in the range [first,last) which does not compare less than val.
        auto cmp = [](hotstr& a, hotstr& b) { return a.first == b.first ? strcmp(a.second,b.second) < 0 : a.first > b.first; };
        priority_queue<hotstr,vector<hotstr>,decltype(cmp)> minheap(cmp);
        if ((l = lower_bound(l, r, userString)) == r) return {}; // nothing found case
        for (r = l; r != sentences.end() && !strncmp(r->c_str(), userString.c_str(), userString.length()); ++r) {
            // Pass through a 3-size minheap, keeping hottest at bottom, popping off coldest at top
            minheap.emplace(hotmap[*r], r->c_str());
            if (minheap.size() > 3) minheap.pop();
        }

        vector<string> results;
        while (minheap.size())
            results.push_back(minheap.top().second), minheap.pop();
        std::reverse(results.begin(), results.end());
        return results;
    }
    
    void insert(string& str) {
        auto existsIter = hotmap.find(str);
        if (existsIter != hotmap.end()) {
            existsIter->second++;
        } else {
            // upper_bound: Iterator to the first element in the range [first,last) which compares greater than val.
            auto insertIter = upper_bound(sentences.begin(), sentences.end(), str);
            sentences.insert(insertIter, str);
            hotmap[str] = 1;
        }
    }
};

/**
 * Your AutocompleteSystem object will be instantiated and called as such:
 * AutocompleteSystem obj = new AutocompleteSystem(sentences, times);
 * List<String> param_1 = obj.input(c);
 */
```