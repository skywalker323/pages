# 140 Word Break II – Hard

### Problem:

Given a string s and a dictionary of words dict, add spaces in s to construct a sentence where each word is a valid dictionary word.

Return all such possible sentences.

For example, given  
s = “catsanddog”,  
dict = \[“cat”, “cats”, “and”, “sand”, “dog”\].

A solution is \[“cats and dog”, “cat sand dog”\].

### Thoughts:

A easily modified version from Word Break I shown below is having Time Limit Exceeded issue when you submit.  
Probably this is because string manipulation is taking more time comparing just only keep a record of index.

### Solutions:

easier to understand version

```java
public class Solution {
    public List<String> wordBreak(String s, List<String> wordDict) {
        List<String> result = new LinkedList<String>();
        if (s == null || s.length() == 0 || wordDict == null) {
            return result;
        }
        HashMap<Integer, List<Integer>> pointers = new HashMap<Integer, List<Integer>>();
        List<Integer> tmp = new LinkedList<Integer>();
        tmp.add(-1);
        pointers.put(-1, tmp);
        for (int j = 0; j < s.length(); j ++) {
            pointers.put(j, new LinkedList<Integer>());
            for (int i = 0; i <= j; i ++) {
                if (pointers.get(i - 1).size() > 0 && wordDict.contains(s.substring(i, j + 1))) {
                    pointers.get(j).add(i);
                }
            }
        }
        generate(s, result, s.length() - 1, pointers, "");
        return result;
    }
    private void generate(String s, List<String> result, int index, HashMap<Integer, List<Integer>> pointers, String suffix) {
        List<Integer> nexts = pointers.get(index);
        if (pointers.size() == 0) {
            return;
        }
        if (index == -1) {
            result.add(suffix.substring(0, suffix.length() - 1));
            return;
        }
        for (Integer next:nexts) {
            generate(s, result, next - 1, pointers, s.substring(next, index + 1) + " " + suffix);
        }
    }
}
```

First version:

```cpp
 vector <string> v; // global vector
 void compute (string s, vector <string> wd, int idx, string temp){
         
           if (idx >= s.length()){ // base case
               
               temp.pop_back(); // backtracking 
               v.push_back(temp);
           }
            string str;
           for (int i=idx; i< s.length(); i++){
                 str+= s[i]; // check every character 
               if (find(wd.begin(), wd.end(), str)!= wd.end()){ // using find fn. to check word present or not in dict
                   
                  compute(s, wd, i+1, temp+str+' '); // r.c 
              }
           }
      }
    vector<string> wordBreak(string s, vector<string>& w){
        
       compute (s, w, 0, "");
          return v;
    }
```



