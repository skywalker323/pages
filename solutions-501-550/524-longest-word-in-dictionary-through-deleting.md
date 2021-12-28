# 524 Longest Word in Dictionary through Deleting

### Problem:

Given a string and a string dictionary, find the longest string in the dictionary that can be formed by deleting some characters of the given string. If there are more than one possible results, return the longest word with the smallest lexicographical order. If there is no possible result, return the empty string.

Example 1:

```
Input:
s = "abpcplea", d = ["ale","apple","monkey","plea"]

Output: 
"apple"
```

Example 2:

```
Input:
s = "abpcplea", d = ["a","b","c"]

Output: 
"a"
```

Note:  
1. All the strings in the input will only contain lower-case letters.  
2. The size of the dictionary won't exceed 1,000.  
3. The length of all the strings in the input won't exceed 1,000.

### Solutions:

```java
Iterate the dictionary and at each iteration check if d[i] is present within input string s, using 
the two-pointer approach.


string curr=d[i];
int a=0,b=0;
string temp="";
int cc=0; // character count
while(a<m && b<curr.length()){
	if(s[a]==curr[b]){
		++cc;
		++b;
	}
	++a;
}
If d[i] is present completely within s:
Then check if the length of the curr string is greater than our ans string, if so update ans, that is ans = curr.
Also, if the length of curr string is equal to ans and curr is lexicographically smaller than ans, then update ans with curr, that is ans=curr.
// current word is completely present in "s"
if(cc==curr.length()){ 
	// check whether "curr" is longer than "ans"
	// OR
	// if "curr" and "ans" are same length then choose the lexicographically smaller one
	if(ans.length()<curr.length() || (ans.length()==curr.length() && curr<ans)){
		ans=curr;
	}
}
Finally, return ans.
COMPLETE CODE

class Solution {
public:
    string findLongestWord(string s, vector<string>& d) {
        int n=d.size();
        int m=s.length();
        string ans="";
        for(int i=0;i<n;++i){
            
            // checking weather d[i] is present within "s" using two pointer approach
            string curr=d[i];
            int a=0,b=0;
            string temp="";
            int cc=0; // character count
            while(a<m && b<curr.length()){
                if(s[a]==curr[b]){
                    ++cc;
                    ++b;
                }
                ++a;
            }
            // current word is completely present in "s"
            if(cc==curr.length()){ 
                // check whether "curr" is longer than "ans"
                // OR
                // if "curr" and "ans" are same length then choose the lexicographically smaller one
                if(ans.length()<curr.length() || (ans.length()==curr.length() && curr<ans)){
                    ans=curr;
                }
            }
        }
        
        return ans;
    }
};
```

```java
public class Solution {
    public String findLongestWord(String s, List<String> d) {
        String result = "";
        for (String str:d) {
            if (str.length() > s.length()) {
                continue;
            }
            int i = 0;
            for (int j = 0; j < s.length(); j ++) {
                if (i < str.length() && s.charAt(j) == str.charAt(i)) {
                    i ++;
                }
            }
            if (i == str.length() && str.length() >= result.length()) {
                if (str.length() > result.length() || str.compareTo(result) < 0) {
                    result = str;
                }
            }
        }
        return result;
    }
}
```



