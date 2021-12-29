# 320 Generalized Abbreviation

### Problem:

Write a function to generate the generalized abbreviations of a word.

Example:  
Given word = "word", return the following list \(order does not matter\):  
\["word", "1ord", "w1rd", "wo1d", "wor1", "2rd", "w2d", "wo2", "1o1d", "1or1", "w1r1", "1o2", "2r1", "3d", "w3", "4"\]

### Solutions:

```java
/*For backtracking problems, we essentially start solving the problem by making a choice out of the many options available and then we validate if that choice satisfies the constraints and then we proceed towards reaching the goal recursively.

In this problem, the choices that we can make for every character to build a final String is to either choose the character as it is or use the corresponding abbreviated option which is 1. When choosing the abbreviated option we need to ensure that we consider the previous continuing count, if any. We make each of these choices recursively until we have explored the last character in the input String.

While writing the algorithm we need to keep track of the following -

Current index (pos) that is being explored in the input String
The String (StringBuilder curr) being constructed
The count of abbreviated options chosen
Code:
*/

class Solution {
    private List<String> result = new ArrayList<>();
	
    public List<String> generateAbbreviations(String word) {
        dfs(word, new StringBuilder(), 0, 0);
        return result;
    }
    
    private void dfs(String word, StringBuilder curr, int pos, int count){
        // Base condition:
        // When the index (pos) has reached the length of the word, that means we already have a word ready to be added to our result
        if(pos == word.length()) {
            // If the count is not 0 then we need to append that count to our current StringBuilder
            if (count > 0) curr.append(count);
            result.add(curr.toString());
            return;
        }
        // Before starting DFS we need to check the original length of the StringBuilder,
        // so that at the end of the dfs call we can reset the StringBuilder to its original value
        int originalLength = curr.length(); 
        
        // DFS for the abbreviation choice
        dfs(word, curr, pos+1, count+1); 
        curr.setLength(originalLength);
        
        // Before doing the DFS call for the character, we are ensuring if there are any counts maintained for the abbreviated choice, 
        // then that should be appended to the current StringBuilder before we append the new character
        curr.append(count > 0 ? count : "").append(word.charAt(pos));
        
        // DFS for the character choice
        dfs(word, curr, pos+1, 0); 
        curr.setLength(originalLength);
    }
}
```

```java
public class Solution {
      public List<String> generateAbbreviations(String word) {
            List<String> result = new LinkedList<String>();
            dfs(result, "", 0, word);
            return result;
      }
      private void dfs(List<String> result, String prefix, int start, String w) {
            result.add(prefix + w.substring(start));
            if (start == w.length()) {
                return;
            }
            int i = start;
            if (start > 0) {
                i = start + 1;
            }
            for (; i < w.length(); i++) {
                String next = prefix + w.substring(start, i);   
                for (int j = 1; j <= w.length() - i; j++) {
                    dfs(result,  next + j, i + j, w);
                }
            }

      }
}
```



