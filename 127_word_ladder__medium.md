# 127 Word Ladder – Medium

### Problem:

Given two words \(beginWord and endWord\), and a dictionary’s word list, find the length of shortest transformation sequence from beginWord to endWord, such that:

Only one letter can be changed at a time  
Each intermediate word must exist in the word list  
For example,

Given:  
beginWord = "hit"  
endWord = "cog"  
wordList = \["hot","dot","dog","lot","log"\]

As one shortest transformation is "hit" -&gt; "hot" -&gt; "dot" -&gt; "dog" -&gt; "cog",  
return its length 5.

Note:

Return 0 if there is no such transformation sequence.  
All words have the same length.  
All words contain only lowercase alphabetic characters.

### Thoughts:

This is a little complicated problem. Solution may vary based on different requirements.

Solution below is using a modified BFS version. It is also modifying the wordDict.

If constraint has cannot modify wordDic, this approach might not work sell. But could use an extra hashSet to achieve the same goal.

### Solutions:

```java
 bool onediff(string &s1, string &s2)
    {
       if(s1.length()!=s2.length()) return false;
       int numdiff=0;
       for(int i=0;i<s1.length();i++)
       {
           if(s1[i]!=s2[i])
           {
               numdiff++;
               if(numdiff>1) return false;
           }
       }
       return  numdiff==1;
    }
    
    int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
        unordered_set<string> words(wordList.begin(),wordList.end());
        if(words.find(endWord)==words.end()) return 0;
        deque<string> store;
        store.push_back(beginWord);
        int steps=1;
        while(store.size()>0)
        {
            int n=store.size();
            steps++;
            while(n--)
            {
                string cur=store.front();store.pop_front();
                vector<string> visited;
                for(auto it=words.begin();it!=words.end();it++)
                {
                   string s=(*it);
                   if(!onediff(s,cur)) continue; 
                   if(s==endWord) return steps;
                   store.push_back(s);
                   visited.push_back(s); 
                }
                for(string s:visited) words.erase(s);
            }
        }
        return 0;             
    }
```



