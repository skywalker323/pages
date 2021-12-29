# 648 Replace Words

### Problem

In English, we have a concept called root, which can be followed by some other words to form another longer word - let's call this word successor. For example, the root an, followed by other, which can form another word another.

Now, given a dictionary consisting of many roots and a sentence. You need to replace all the successor in the sentence with the root forming it. If a successor has many roots can form it, replace it with the root with the shortest length.

You need to output the sentence after the replacement.

Example 1:

```
Input: dict = ["cat", "bat", "rat"]
sentence = "the cattle was rattled by the battery"
Output: "the cat was rat by the bat"
```

Note:  
1. The input will only have lower-case letters.  
2. 1 &lt;= dict words number &lt;= 1000  
3. 1 &lt;= sentence words number &lt;= 1000  
4. 1 &lt;= root length &lt;= 100  
5. &lt;= sentence words length &lt;= 1000

### Solutions

```java
Basic Idea is to store all words in dictonary into a Trie and for word end node, we store index along with flag. 
This index will be used to replace word in sentence

  Ex: dictionary = {cat, bat, rat}, lets take "bat" and insert into Trie.

  So, "bat" is stored as "b -> a -> t {flag = true, index = 1}"  
  -> 't' is end of word, so flag and index of that node is set. 
Now, that we have our Trie of dictionary ready, we need to replace words.

To replace words in sentence, we search() for prefix of word that is present in Trie. It is similar to startsWith() 
in standard Trie Implementation.

If we find a prefix word, we return its index and replace accordingly. Otherwise move ahead with same word as present 
in sentence, without replacing.

Go through the code, it is easy to understand. In case of any doubt, feel free to ask in the comment.

Code:
struct Node {
    Node* arr[26];
    bool flag = false;
    int idx = -1;
    
    bool contains(char ch) {
        return arr[ch-'a'] != NULL;
    }
    
    void put(char ch, Node* newNode) {
        arr[ch-'a'] = newNode;
    }
    
    Node* getNext(char ch) {
        return arr[ch-'a'] ;
    }
    
    void setEnd(int index) {
        flag = true;
        idx = index;
    }
    
    bool getEnd() {
        return flag;
    }
};


class Solution {
public:
    
    void insert(Node* root, string& s, int index) {
        for(auto& ch : s) {
            if(!root->contains(ch)) {
                root->put(ch, new Node());
            }
            root = root->getNext(ch);
        }
        root->setEnd(index);
    }
    
    void buildDictionary(Node* root, vector<string>& dictionary) { 
        int idx = 0;
        for(auto& word : dictionary) {
            insert(root, word, idx);
            idx++;
        }
    }
    
    int search(Node* root, string& s) {
        for(auto& ch : s) {
            if(!root || !root->contains(ch)) return -1;
            root = root->getNext(ch);
            if(root->getEnd()) return root->idx;
        }
        return -1;
    }
    
    string replaceWords(vector<string>& dictionary, string sentence) {
        Node* root = new Node();
        
        buildDictionary(root, dictionary);
        
        int n = sentence.size(), i = 0;
        string replacedSentence = "";
        while(i < n) {
            string word = "";
            int j = i;
            while(j < n && sentence[j] != ' ') {
                word += sentence[j++];
            }
            i = j+1;
            
            int idx = search(root, word);
            
            replacedSentence += (idx != -1) ? dictionary[idx] : word;
            replacedSentence += " ";
        }
        
        replacedSentence.pop_back();
        return replacedSentence;
    }
};
```



