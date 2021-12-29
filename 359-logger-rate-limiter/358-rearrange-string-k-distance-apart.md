# 358 Rearrange String k Distance Apart

### Problem:

Given a non-empty string s and an integer k, rearrange the string such that the same characters are at least distance k from each other.

All input strings are given in lowercase letters. If it is not possible to rearrange the string, return an empty string "".

Example 1:

```
s = "aabbcc", k = 3

Result: "abcabc"

The same letters are at least distance 3 from each other.
```

Example 2:

```
s = "aaabc", k = 3 

Answer: ""

It is not possible to rearrange the string.
```

Example 3:

```
s = "aaadbbcc", k = 2

Answer: "abacabcd"

Another possible answer is: "abcabcda"

The same letters are at least distance 2 from each other.
```

### Solutions:

```cpp
class Solution {
private:
    // Comparator for pair of char and frequency of char
    struct PairCompare {
        bool operator()(const pair<char, int>& lhs, const pair<char, int>& rhs) {
            return lhs.second < rhs.second;
        }
    };
    
public:
    string rearrangeString(string s, int k) {
        // Find frequency of each character in string
        unordered_map<char, int> freqs;
        for (auto c : s) {
            freqs[c]++;
        }
        
        // Put characters in max heap with most frequent char on top.
        priority_queue<pair<char, int>, vector<pair<char, int>>, PairCompare> max_heap;
        for (auto p : freqs) {
            max_heap.push(p);
        }
        
        // Parking lot to hold last used character until we have atleast k characters
        // in this parking lot. It's a queue to mantain the order in which they got
        // added to the queue.
        queue<pair<char, int>> parking;
    
        string result;
        
        // Process each character by there frequency, greedy algorithm to consume
        // element with highest frequency first.
        while (!max_heap.empty()) {
            auto p = max_heap.top();
            max_heap.pop();
            
            // Add this char to output and put it in parking lot.
            result += p.first;
            p.second--;
            parking.push(p);
            
            // If this parking lot reached the size k, it can start letting out
            // characters one by one after reaching back into max heap.
            if (parking.size() >= k) {
                auto out = parking.front();
                parking.pop();
                if (out.second > 0) {
                    max_heap.push(out);
                }
            }
        }
        
        // If we got a solution consuming all characters of original string, it's a valid
        // reaarangement.
        return result.length() == s.length() ? result : "";
    }
};
```

```java
public class Solution {
    public String rearrangeString(String s, int k) {
        if (k == 0) {
            return s;
        }
        int[] count = new int[26];
        for(int i=0; i < s.length(); i++){
            count[s.charAt(i) - 'a'] ++;
        }
        PriorityQueue<Character> q = new PriorityQueue<Character>(new Comparator<Character>(){
            public int compare(Character c1, Character c2){
                if (count[c1 - 'a'] != count[c2 - 'a']) {
                    return count[c2 - 'a'] - count[c1 - 'a'];
                }
                else {
                    return c1 - c2;
                }
            }
        });

        for (int i = 0; i < 26; i ++) {
            if (count[i] != 0) {
                q.add((char) (i + 'a'));
            }
        }
        StringBuilder sb = new StringBuilder();
        int len = s.length();
        while(!q.isEmpty()){
            int cnt = Math.min(k, len);
            ArrayList<Character> tmp = new ArrayList<Character>();
            for(int i=0; i < cnt; i++){
                if(q.isEmpty())
                    return "";
                char c = q.poll();
                sb.append(c);
                count[c - 'a'] --;
                if(count[c - 'a'] > 0){
                    tmp.add(c);
                }
                len--;
            }

            for(char c: tmp)
                q.add(c);
        }

        return sb.toString();
    }
}
```



