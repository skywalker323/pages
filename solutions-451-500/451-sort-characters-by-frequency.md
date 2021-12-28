# 451 Sort Characters By Frequency

### Problem:

Given a string, sort it in decreasing order based on the frequency of characters.

Example 1:

```
Input:
"tree"

Output:
"eert"

Explanation:
'e' appears twice while 'r' and 't' both appear once.
So 'e' must appear before both 'r' and 't'. Therefore "eetr" is also a valid answer.
```

Example 2:

```
Input:
"cccaaa"

Output:
"cccaaa"

Explanation:
Both 'c' and 'a' appear three times, so "aaaccc" is also a valid answer.
Note that "cacaca" is incorrect, as the same characters must be together.
```

Example 3:

```
Input:
"Aabb"

Output:
"bbAa"

Explanation:
"bbaA" is also a valid answer, but "Aabb" is incorrect.
Note that 'A' and 'a' are treated as two different characters.
```

### Solutions:

```cpp
Solution I - Using Priority Queue
First, we count the frequency of each letter using unordered_map freq.
Then, to sort the frequencies, we use a priority_queue q with pairs: { freq, char }.
Consrtucting the result is simple: just pop each pair from the queue and add that number of chars to res.

class Solution {
public:
    string frequencySort(string s) {
        unordered_map<char, int> freq;
        for (auto a : s) freq[a]++;
        
        priority_queue<pair<int, char>> q;
        for (auto [a, frq] : freq) q.push({frq, a});
        
        string res;
        pair<int, char> curr;
        while (!q.empty()) {
            curr = q.top();
            q.pop();
            res += string(curr.first, curr.second);
        }
        
        return res;
    }
};
Solution II - Using Bucket Sort
First, we count the frequency of each letter using unordered_map freq.
Then, we use a vector to store buckets by the frequency. In each bucket we have a vector of the chars with that frequency.
Constructing the result is simply looping the buckets from the end - to get the higher frequencies first, and adding the chars in the current frequency.

class Solution {
public:
    string frequencySort(string s) {
        unordered_map<char, int> freq;
        for (auto a : s) freq[a]++;
        
        vector<vector<char>> buckets(s.size()+1);
        for (auto [a, frq] : freq) buckets[frq].push_back(a);
        
        string res;
        for (int i = s.size(); i > 0; i--) {
            for (auto ch : buckets[i]) {
                res += string(i, ch);
            }
        }
        
        return res;
    }
};
```



