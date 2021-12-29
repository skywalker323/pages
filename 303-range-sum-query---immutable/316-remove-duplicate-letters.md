# 316. Remove Duplicate Letters

### Problem:

Given a string which contains only lowercase letters, remove duplicate letters so that every letter appear once and only once. You must make sure your result is the smallest in lexicographical order among all possible results.

Example:  
Given "bcabc"  
Return "abc"

Given "cbacdcbc"  
Return "acdb"

### Solutions:

```java
Basically a monotonic stack, where we have to take care that we do not remove an element which can never be found again, as we have to include each element exactly once.

    string removeDuplicateLetters(string s) {
        string res;        
        unordered_map <char, int> counts;
        for (char c : s)
            counts[c] ++;        
        
        
        for (auto& c : s){
            if (--counts[c] == 0)
                counts.erase(c);                
     
            /*If the character is already done, we can just continue*/
            if (count(res.begin(), res.end(), c) > 0)
                continue;
            
            /*
            Each candidate tries to prove to be a better candidate
            It tries to remove all others which were bigger than him
            
            BUT, we can only remove a bigger candidate if we know that 
            he can occur again in future, as we have to include everyone only once (eg, bcbca)
            
            */
            while (!res.empty() && res.back() > c && (counts.find(res.back()) != counts.end() && counts[res.back()] >= 1))
                res.pop_back();
            
            res.push_back(c);
        }
        return res;
    }
```

```java
public class Solution {
    public String removeDuplicateLetters(String s) {
        Stack<Character> stack = new Stack<Character>();
        int[] count = new int[26];
        boolean[] visited = new boolean[26];
        for (int i = 0; i < s.length(); i ++) {
            count[s.charAt(i) - 'a'] ++;
        }
        for (int i = 0; i < s.length(); i ++) {
            char c = s.charAt(i);
            count[c - 'a'] --;
            if (visited[c - 'a'] == true) {
                continue;
            }
            while (!stack.isEmpty() && c < stack.peek() && count[stack.peek() - 'a'] > 0) {
                visited[stack.peek() - 'a'] = false;
                stack.pop();
            }
            stack.push(c);
            visited[c - 'a'] = true;
        }

        StringBuilder sb = new StringBuilder();
        while (!stack.isEmpty()) {
            sb.insert(0, stack.pop());
        }
        return sb.toString();
    }
}
```



